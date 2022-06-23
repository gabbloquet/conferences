# LM Ecommerce Website

[Speaker](https://www.linkedin.com/in/thomas-rumas/)

`f2e-nest-svelte`  et `f2e-portal` repos

We render html on server side with Java, we replace dom by another.

No big problems but need **rigors**.

## Since 2 years

VueJS with NuxtJS ?
TTFB fois 1.5, performance problems

Svelte as template engine + NestJS on server-side ?
La meme chose que Java avant

Sapper deprecated, test Sveltetkit
head html for hydration, body develop by community in the futur ?

Svelte on SSR ? 
 - CSR with hydratation
 - The views with SSR

Hydratation > Using client side javascript to add application state and interactivity to server rendered html.

```javascript
// dans le root du server
app.engine('js', svelteTemplateEngine)

// retourne la bonne vue en fonction du path
svelteTemplateEngine(filePath, options){
	
}
```

Dans rollup on a donc 2 conf, une pour le CSS, une pour le SSR. On met l'hydratation des 2 cotes.

> Full masterize of the technical stack, personalization top
> Lighter than VueJS
> NodeJS working on only on CPU thread but with PM2 > cluster mode 
> CSR : Google web vitals and even lazy-loading JS and CSS file when component is showing on client viewport

