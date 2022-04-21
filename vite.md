# Vite

Bonne intro où ils donnent leurs avis (go). Débutant.

On a la partie dev : esbuild  
La partie build : rollup.

## Dev

Esbuild (remplace babel), qui comprend javascript et typescript _blazing fast_.
Au premier build il preload. Et boom ! Performance.

Il optimise le reload, ne recalculer que le fichier changer.

### Pros

On veut Profiter du **three shaking** et du **code splitting**. Et ça on le fait nativement avec Vite.
Avec React.lazy(), on import dynamiquement nos composants, en changeant on voit qu'il va créer un bundle par page, et quand on charge la page il n'y a que ce bundle préci de chargé.  

 - SASS, PostCss géré seul aussi.
 - SSR bien foutu aussi.
 
On peut basculer facile sur vite avec l'ajout du `viteconfig` si on a un react legacy.

### Outsiders

Vite Browser, RomeJS (le tout en un), swc, astro (par snowpack), vitest, webpack.

### Opinion

GO !
=> Utiliser par nuxt, prod ready, developer experience ouf, même en scalant on a tjs une bonne perf...