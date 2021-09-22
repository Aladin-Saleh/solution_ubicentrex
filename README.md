# solution_ubicentrex
Solution en javascript permettant d'isoler les adresses via le script de base fournie par ubicentrex.


Ce script permet d'isoler les adresses dans le calendrier de prise de rendez-vous.
Normalement, vous devez être en possession d'un bout de code en javascript qu'ubicentrex vous fournit.

Voici ce que vous avez :
```html
<div style='width: 100%;'>
<a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=xxxxx&nsoc=yyyyy'
webagcli-id=64078289 webagsoc-id =32802 >Prendre rendez-vous par Internet</a>
<script type='text/javascript'>// <![CDATA[
!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id))
{js=d.createElement(s);js.id=id;js.src=p+'://secure.ubicentrex.net/webagdispo.php';fjs.parentNode.insertBefore(js,fjs);}}(document,'script','webag-js');
// ]]></script>
</div>
```

Le code dans ce format n'est pas très lisible.
Voici le même code mais rendu plus propre et plus lisible.

## Le code HTML 
```html
    <div style='width: 100%;'>
            <a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=64078289&nsoc=32802' webagcli-id=64078289 webagsoc-id =32802 >Prendre rendez-vous par Internet</a>
                <script type='text/javascript'>
                  ....
                </script>
    </div>
```


## Le script javascript natif

```JS
!function(d, s, id) {
var js;
var fjs = d.getElementsByTagName(s)[0];
var p = /^http:/.test(d.location) ? "http" : "https";
if (!d.getElementById(id)) {
js = d.createElement(s);
js.id = id;
js.src = p + "://secure.ubicentrex.net/webagdispo.php";
fjs.parentNode.insertBefore(js, fjs);
}
}(document, "script", "webag-js");

```

## Le code global 

```html

<div style='width: 100%;'>
    <a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=64078289&nsoc=32802'
    webagcli-id=64078289 webagsoc-id =32802 >Prendre rendez-vous par Internet</a>
    <script type='text/javascript'>
        !function(d, s, id) {
                var js;
                var fjs = d.getElementsByTagName(s)[0];
                var p = /^http:/.test(d.location) ? "http" : "https";
                if (!d.getElementById(id)) {
                js = d.createElement(s);
                js.id = id;
                js.src = p + "://secure.ubicentrex.net/webagdispo.php";
                fjs.parentNode.insertBefore(js, fjs);
                }
                }(document, "script", "webag-js");


    </script>
</div>
```

Voici le resultat que vous avez sur votre page web avec le code suivant : 

![Capture d’écran 2021-05-17 093955](https://user-images.githubusercontent.com/67257097/118453118-2773a600-b6f7-11eb-9899-8f71a4906d6e.png)

Comme vous pouvez le voir, le calendrier affiche les creneaux de tout le monde.
De plus, une liste déroulante donne accès aux crénaux de tout le monde.
Cela ne nous arrange pas pour des raisons de confidentialité (par exemple).

Nous allons donc modifier ce code pour empecher l'accès aux crénaux de tout le monde.
Pour se faire nous allons : 

-   Supprimer la liste déroulante.

-   Ajouter un paramètre dans l'URL pour chaque adresse.


## Supprimer la liste déroulante.
Il s'agit de l'étape la plus simple.
Nous allons récuperer les classes de la liste déroulante et les rendre invisible via du CSS.

### Le code CSS : 
```CSS
        body,.liste_choix_wrapper,.lspecialite liste_choix,.liste_choix_wrapper,.check-icon-wrapper {display: none;}
```
Pour résumer, ce code permet de ne pas afficher le contenu indiqué sur la page.
Nous allons rendre le body "invisible" pour une raison que nous verrons plus tard.

Vous pouvez copier ce code et l'insérer dans votre page : 
```html
<style>
        body,.liste_choix_wrapper,.lspecialite liste_choix,.liste_choix_wrapper,.check-icon-wrapper {display: none;}
</style>
```

A ce stade votre code doit ressembler à cela : 
```html
         
<style>
        body,.liste_choix_wrapper,.lspecialite liste_choix,.liste_choix_wrapper,.check-icon-wrapper {display: none;}
</style>

<div style='width: 100%;'>
    <a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=64078289&nsoc=32802'
    webagcli-id=64078289 webagsoc-id =32802 >Prendre rendez-vous par Internet</a>
    <script type='text/javascript'>
        !function(d, s, id) {
                var js;
                var fjs = d.getElementsByTagName(s)[0];
                var p = /^http:/.test(d.location) ? "http" : "https";
                if (!d.getElementById(id)) {
                js = d.createElement(s);
                js.id = id;
                js.src = p + "://secure.ubicentrex.net/webagdispo.php";
                fjs.parentNode.insertBefore(js, fjs);
                }
                }(document, "script", "webag-js");


    </script>
</div>
```


Il est maintenant temps de nous attaquer au script Javascript.
Comme ennoncé plus tôt, nous allons passer l'adresse via l'URL en lui indiquant un paramètre.


## Récuperer le paramètre (l'adresse) depuis l'URL

Initialement, pour accéder à la page il suffisait d'entrer une URL sans paramètre tel que :

![image](https://user-images.githubusercontent.com/67257097/118457248-dd3ff400-b6f9-11eb-8217-61c20955691d.png)

Notre objectif va être de récuperer le paramètre fourni dans l'URL pour afficher les crénaux d'une adresse.
Voici à quoi ressemblera notre ULR : 

![image](https://user-images.githubusercontent.com/67257097/118457622-3b6cd700-b6fa-11eb-809b-354b8881956d.png)

Maintenant, voyons le code qui va nous permettre de faire cela.
Il s'agit d'une fonction qui tient sur 3 lignes.

```JS 
document.getElementById("title").innerHTML = new URL(document.location.href).searchParams.get("addresse");
```
Cette ligne permet d'afficher le nom de l'adresse en haut du calendrier.

Au préalable on aura déclaré une balise html telle quel : 
```html5
    <h3 style="text-align: center;" id="title"></h3>
```

### Le code : 

```JS
function get_url_settings() {   
    document.getElementById("title").innerHTML = new URL(document.location.href).searchParams.get("addresse");      
        return new URL(document.location.href).searchParams.get("addresse");
}
```

## Affichage du  calendrier de crénaux

Maintenant que nous avons récuperé l'adresse via le paramètre dans l'URL, attaquons nous à la partie la plus compliquée du code.
Nous allons afficher les crénaux de l'adresse dans le calendrier.

Voici le code qui va nous permettre de faire cela : 

```JS

                
                function setAddr(addr) {
                    const element = document.getElementById("owebagdispopreladr");
                    isFound = false;
                    for (let index = 0; index < document.getElementsByTagName("option").length; index++) {
                        if (document.getElementsByTagName("option")[index].text == get_url_settings() ) {
                            element.value = document.getElementsByTagName("option")[index].value;
                            owebagdispo.fsetAdr(element.value);
                            isFound = true;
                            document.body.style.display = "block"

                        }
                    }
                    if (!isFound) {
                        console.log("Not found !");
                        document.getElementById("owebagdispoprecadreagd").style.display = "none";
                        document.body.innerHTML = "<h3 style='text-align: center;'>Il semblerait que l'addresse fournit n'existe pas (ou plus).<br>Veuillez verifier que l'addresse a été ecrite correctement ou contacter l'hébergeur de la plateforme.</h3>"
                        //Ajouter le lien pour retourner en page d'accueil
                    }
                }
            
            
```
            
Décortiquons ce code qui peut faire peur au premier abord.

#### Le code qui permet de vérifier l'existence de l'adresse.
```JS
                    const element = document.getElementById("owebagdispopreladr");
                    isFound = false;//boolean
                    for (let index = 0; index < document.getElementsByTagName("option").length; index++) {
                        if (document.getElementsByTagName("option")[index].text == get_url_settings() ) {
                            element.value = document.getElementsByTagName("option")[index].value;
                            owebagdispo.fsetAdr(element.value);
                            isFound = true;
                            document.body.style.display = "block"
                        }
                    }
```

Nous allons d'abord récuperer l'ID de la liste déroulante pour pouvoir interragir avec cet ID.
On la stocke dans une constante que l'on appelle "element".
Cela va nous permettre de rendre notre code plus propre, plus court et plus rapide.

On déclare ensuite un boolean "isFound".
Ce boolean va nous permettre de gérer les erreurs.
Sa valeur de base (etat) est définie à "false" (faux).
```JS
                    const element = document.getElementById("owebagdispopreladr");
                    isFound = false;//boolean
```

Une fois les variables et constantes déclarées, on peut créer notre boucle.
Cette boucle va récuperer tous les éléments de la liste déroulante que nous avons rendue "invisible" plus tôt.
Elle va les "lire" et les comparer avec la valeur du paramètre fourni.
Celle-ci continue de tourner jusqu'à ce que : 

-   La condition soit remplie, c'est à dire qu'une adresse à été trouvée.
        Dans ce  cas là :
        
            - On recupère la valeur de la liste 
            - On fait appel à une fonction d'ubicentrex qui va confirmer l'adresse.
            - On change la valeur (etat) de notre boolean à "true" (vrai).
            - On affiche le calendrier avec les crénaux de l'adresse.

-   La condition n'a pas été remplie, c'est à dire qu'aucune adresse n'a été trouvée.
        Dans ce cas là : 
        
            - Notre boolean ne change pas d'etat.
            - Le programme execute le code qui va gérer l'erreur

```JS
 for (let index = 0; index < document.getElementsByTagName("option").length; index++) {
                        if (document.getElementsByTagName("option")[index].text == get_url_settings() ) {
                            element.value = document.getElementsByTagName("option")[index].value;
                            owebagdispo.fsetAdr(element.value);
                            isFound = true;
                            document.body.style.display = "block"
                        }
                    }
```

#### Le code qui permet de gérer le cas où le paramètre fourni est incorrect : 

```JS
if (!isFound) {
                        document.getElementById("owebagdispoprecadreagd").style.display = "none";
                        document.body.innerHTML = "<h3 style='text-align: center;'>Il semblerait que l'adresse fournit n'existe pas (ou plus).<br>Veuillez verifier que l'adresse a été ecrite correctement ou contacter l'hébergeur de la plateforme.</h3>"
                        //Ajouter le lien pour retourner en page d'accueil
                    }
```
Dans le cas où notre boolean n'a pas changé d'etat (que l'adresse n'existe pas ou bien mal ecrite) un message d'erreur sera affiché sur la page.

![image](https://user-images.githubusercontent.com/67257097/118462156-ac15f280-b6fe-11eb-9072-d49b008a2078.png)


Nous nous approchons de la fin.
Il ne reste plus que quelques lignes de code à entrer.
En effet, pour que notre code soit fonctionnel il faut que nous gérions l'asynchronisme.
```
L'asynchronisme désigne le caractère de ce qui ne se passe pas à la même vitesse, que ce soit dans le temps ou dans la vitesse proprement dite, par opposition à un phénomène synchrone.
```

## Gérer l'asynchronisme

Pour gérer l'asynchronisme nous allons insérer la fonction setAddr(addr) que nous venons de créer dans une autre fonction propre au javascript qui va nous permettre de gérer nos problèmes d'asynchronisme.

```JS
setTimeout(action_a_executer, delai_execution);
```
Voici à quoi devrait ressembler le code :

```JS
setTimeout(() => { setAddr(get_url_settings());}, 2000); 
```

Il existe d'autres façons de gérer l'asynchronisme tel que : 

Async/Await et les Promise (promesse):

Exemple de code : 

```JS
async function ma_fonction_asynchrone();
const ma_constante = await ma_fonction_asynchrone();
```

Toutefois, notre problème ne requiert pas de les utiliser.
Libre à vous de vous documenter : https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/async_function

Si vous avez correctement suivi le guide, voici à quoi devrait ressembler le code : 
```html5
 <style >
        body,.liste_choix_wrapper,.lspecialite liste_choix,.liste_choix_wrapper,.check-icon-wrapper {display: none;}
    </style>

    <div style='width: 100%;'>
        <h3 style="text-align: center;" id="title"></h3>
        <a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=64078289&nsoc=32802'
        webagcli-id=64078289 webagsoc-id =32802 >Prendre rendez-vous par Internet</a>
        <script type='text/javascript'>
        function get_url_settings() {   
        document.getElementById("title").innerHTML = new URL(document.location.href).searchParams.get("addresse");
        return new URL(document.location.href).searchParams.get("addresse");
        }

        (function(d, s, id) {
        var js;
        var fjs = d.getElementsByTagName(s)[0];
        var p = /^http:/.test(d.location) ? "http" : "https";
                if (!d.getElementById(id)) {
                    js = d.createElement(s);
                    js.id = id;
                    js.src = p + "://secure.ubicentrex.net/webagdispo.php";
                    fjs.parentNode.insertBefore(js, fjs);
                }

        })(document, "script", "webag-js");
                setTimeout(() => { setAddr(get_url_settings());
            }, 2000);        
                setAddr(get_url_settings());
                function setAddr(addr) {
                    const element = document.getElementById("owebagdispopreladr");
                    isFound = false;
                    for (let index = 0; index < document.getElementsByTagName("option").length; index++) {
                        if (document.getElementsByTagName("option")[index].text == get_url_settings() ) {
                            element.value = document.getElementsByTagName("option")[index].value;
                            owebagdispo.fsetAdr(element.value);
                            isFound = true;
                            document.body.style.display = "block"
                        }
                    }
                    if (!isFound) {
                        console.log("Not found !");
                        document.getElementById("owebagdispoprecadreagd").style.display = "none";
                        document.body.innerHTML = "<h3 style='text-align: center;'>Il semblerait que l'adresse fournit n'existe pas (ou plus).<br>Veuillez verifier que l'addresse a été ecrite correctement ou contacter l'hébergeur de la plateforme.</h3>"
                        //Ajouter le lien pour retourner en page d'accueil
                    }
                }
           // }   
        // ]]></script>
        </div>
```
Voici à quoi devrait ressemblait la page web : 

![Capture d’écran 2021-05-17 115218](https://user-images.githubusercontent.com/67257097/118469645-2e55e500-b706-11eb-82c0-413563c4178e.png)





Si vous rencontrez des problèmes, merci de bien vouloir re-lire le guide :) .

Aladin SALEH



