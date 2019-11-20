<script>
	import { onMount } from 'svelte'

	import art from './art'

	let initialValue =
`String.prototype.mySubstring = function(a, b = this.length) {
  const clamp = val => Math.max(0, Math.min(val, this.length));

  let subString = "",
          start = clamp(a),
            end = clamp(b);

  if (start > end) {
    [start, end] = [end, start];
  }

  for (let i = start; i < end; i++) {
    subString += this[i];
  }

  return subString;
};

console.log("hello world!".mySubstring(1, 5));

console.log("hello world!".mySubstring(6));


`

	let value = initialValue, result = '', codeElement;

	let superEpicFunction = n => {
		if (isNaN(n) || n >= art.length) {
			console.log(`<span style="color: #E53E3E;">'superEpicFunction' needs a number; not negative, and less than ${ art.length }.</span>`)
		}
		else if (n < art.length) {
			console.log(art[n].replace(/ /g, '&nbsp;').split('\n').join('<br />'));
		}
	}

	superEpicFunction()

	$: {
		try {
			eval(value)
		}
		catch(error) {
			result += `<span style="color:#E53E3E;">${ error }</span>`
		}
	}

  console.log = (...args) => result += `> ${ args.join(' ') }<br />`

  const selectEnd = el => {
  	let text = el.firstChild;

  	let selection = window.getSelection(),
            range = document.createRange();

		range.setEnd(text, text.length);

		range.collapse(false)
		selection.removeAllRanges();
		selection.addRange(range);
  }

	onMount(() => {
		selectEnd(codeElement)

		codeElement.addEventListener('input', ({ target }) => {
			result = ''

			value = target.textContent
		})
	})
</script>

<main>
	<h1><a href="https://cassidoo.co" target="_blank">Cassidoo's</a> <code>mySubstring</code></h1>
	<p>
		Stay safe, <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval" target="_blank">don't <code>eval</code></a> and try the <code>superEpicFunction(n: { new Array(art.length).fill(0).map((n, i) => i).join('|') })</code>.
	</p>
	<pre>
		<code contenteditable="true" spellcheck="false" bind:this={ codeElement }>
			{ initialValue }
		</code>
	</pre>

	<div id="result">
		<code>
			<em>{@html result }</em>
		</code>
	</div>

	<small>
		Created by <a href="https://twitter.com/nbgoodall" target="_blank">Nick</a>.
	</small>
</main>

<style>
	main {
		max-width: 700px;
		margin: 0 auto;
	}

	pre, #result {
		padding: 10px 15px;
		border-radius: 10px;
	}

	#result {
		background-color: #E2E8F0;

		margin-bottom: 20px;
	}

	pre {
		min-height: 200px;
		overflow: scroll;
		background-color: #000;
		color: #20C20E !important;
		text-align: left;
		margin-bottom: 10px;
	}

	code {
		font-family: Courier, monospace;
	}

	pre code, #result code {
		width: 100%;
		height: 100%;
		display: block;

		background-color: transparent !important;

		line-height: 20px;

		font-size: 16px;
	}

	pre code :global(span) {
		background-color: transparent !important;

		color: #20C20E !important;
	}

	code:focus {
		outline: none;
	}

	h1 code, p code {
		padding: 2px .3em;
		border-radius: 2px;
		background-color: #E2E8F0;
	}

	p {
		line-height: 24px;
	}
</style>