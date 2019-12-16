# Introduction

- Objectif initial de JavaScript: Rendre le web plus rapide en faisant par exemple la vérification des formulaires sur le client (évite de re-télécharger la page).

# I. Loading and Execution

## Script positioning

- Tag `script` bloque le browser pour l'éxécuter

  **&rarr; Placer les scripts à la fin du body**

- Pour télécharger un script, il faut faire une requète HTTP &rarr; surcout.

  **&rarr; Grouper les scripts (voir webpack)**

## Nonblocking Scripts

- L'attribut `defer` peut être ajouté à un tag script. Le browser commence à télécharger le script quand il rencontre le tag mais ne bloque pas. Le script est éxécuté quand la page a fini de charger.

  - `defer` n'est pas supporté par tous les browsers.

- Dynamic script element: ajoute dynamiquement un tag script à la page.
- XMLHttpRequest: télécharge le contenu du script avec XHR object et l'ajoute dans un nouveau tag script. Attention: problème CORS

**Dynamic script element est la meilleure solution car supporté pas tous les browsers et pas besoin de CORS.**

- Exemple:
```js
function loadScript(url, cb) {
  const script = document.createElement('script')
  script.type = 'text/javascript'

  // IE ne supporte que readystatechange
  if (script.readyState) {
    script.onreadystatechange = function () {
      if (script.readyState == 'loaded' || script.readyState == 'complete') {
        script.onreadystate = null
        cb()
      }
    }
  } else {
    script.onload = function() {
      cb()
    }
  }

  script.src = url
  document.getElementByTagName('head')[0].appendChild(script)
}
```

Une fois ce script chargé et exécuté, il est possible d'accéder à ses variables globales dans d'autres scripts.

```html
<script type="text/javascript" src="loader.js"></script>
<script type="text/javascript">
  loadScript("the-rest.js", function(){
    Application.init();
  });
</script>
```

- Il vaut mieux grouper ces deux scripts, les minifier et les inclure dans la page HTML.

&rarr; Voir LazyLoad et LABj
