# solution_ubicentrex
Solution en javascript permettant d'isoler les adresses via le script de base fournit par ubicentrex.


Ce script permet d'isoler les adresses dans le calendrier de prise de rendez-vous.
Normalement, vous devez être en possession d'un bout de code en javascript qu'ubicentrex vous fournit.

Voici ce que vous avez :
```html
<div style='width: 100%;'>
<a class='webagdispo_frame' href='https://secure.ubicentrex.net/dispo_medecin.php?n=64078289&nsoc=32802'
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
