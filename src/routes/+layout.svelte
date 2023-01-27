<script lang='ts'>
	import '@skeletonlabs/skeleton/themes/theme-hamlindigo.css'
	import '@skeletonlabs/skeleton/styles/all.css'
	import '../app.postcss'
	import { touchDevice, mathliveReady, mathfieldElement } from '$lib/stores'
	import { onMount } from 'svelte'
    import { getLogger } from '$lib/utils'
	import type { MathfieldElement } from 'mathlive'

    let { info, fail, warn } = getLogger('Global layout', 'info')
    info('-- Initialization --')
	// info(dev ? '. developpement version' : '. production version')
	
    onMount(() => {
		// detects a touche screen device at the first touch
		//  https://codeburst.io/the-only-way-to-detect-touch-with-javascript-7791a3346685
		window.addEventListener(
			'touchstart',
			function onFirstTouch() {
				touchDevice.set(true)
				window.removeEventListener('touchstart', onFirstTouch, false)
			},
			false,
		)

		import('mathlive')
			.then((m) => {
				mathliveReady.set(true)
				console.log('m', m)
				mathfieldElement.set(m.MathfieldElement)
				// toMarkup.set(m.convertLatexToMarkup)
				// const regex = /\$\$(.*?)\$\$/g
				// const replacement = (_, p1) => m.convertLatexToMarkup(p1)
				// const _formatToHtml = (o) => {
				// 	if (!o) {
				// 		return ''
				// 	}

				// 	if (Array.isArray(o)) {
				// 		return o.map((elmt) => _formatToHtml(elmt))
				// 	} else if (o.text) {
				// 		return { ...o, text: _formatToHtml(o.text) }
				// 	} else if (typeof o === 'string') {
				// 		return o.replace(regex, replacement)
				// 	} else {
				// 		return o
				// 	}
				// }
				// formatToHtml.set(_formatToHtml)
			})
			.catch((e) => {
				fail('erreur while importing mathlive', e)
			})
	})

    

	$: console.log('touchDevice :', $touchDevice)
</script>

<svelte:head>
	<title>UbuMaths - Les maths de la chandelle verte</title>
</svelte:head>

<slot />
