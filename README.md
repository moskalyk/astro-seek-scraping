```js
const AstraSight = require('./index.js');
const axios = require('axios');
const cheerio = require('cheerio');
const async = require('async');
const containsPlanets = (lookup) => {
	const planets = ['Sun', 'Mercury', 'Venus', 'Earth', 'Mars', 'Jupiter','Saturn', 'Neptune', 'Uranus', 'Pluto']

	for(let planet of planets){
		if(lookup[0] == planet)
			return true
	}
}
const containsAspect = (lookup) => {
	const aspects = [
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-barevny-kvadratura.png', 
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-barevny-konjunkce.png', 
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-barevny-trigon.png',
		'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-barevny-sextil.png'
	]
	for(let aspect of aspects){
		if(lookup == aspect)
			return aspect
	}
}

(async () => {
	const res = await axios('https://horoscopes.astro-seek.com/astrology-aspects-transits-online-calendar')
	const $ = cheerio.load(res.data)

	let view = []
	let temp = []
	let aspectsBuffer = []

	$("table")
	  .find("tr").each((row, ref) => {
		$(ref).find('td').each((row, ref) => {
			const elem = $(ref)
			const text = elem.text().split(' ')
			const img = $(ref).find('img')
			aspectsBuffer.push(containsAspect(img.attr('src')))
			if(containsPlanets(text) && text != undefined){
				console.log(text)
				temp.push(text)
				if(temp.length == 2){
					// console.log(temp)
					view.push(temp)
					temp = []
				}
			}
		})
	  })

	console.log(view)
	const aBuffers = aspectsBuffer.filter((aspect) => { 
		return aspect != undefined 
	})
	console.log(aBuffers)
})()
```

Outputs
```
[
  [ [ 'Mercury' ], [ 'Neptune' ] ],
  [ [ 'Venus' ], [ 'Pluto' ] ],
  [ [ 'Mercury' ], [ 'Neptune' ] ],
  [ [ 'Venus' ], [ 'Pluto' ] ],
  [ [ 'Mercury' ], [ 'Neptune' ] ], <-
  [ [ 'Mercury' ], [ 'Neptune' ] ],
  [ [ 'Venus' ], [ 'Jupiter' ] ],
  [ [ 'Venus' ], [ 'Jupiter' ] ],
  [ [ 'Venus' ], [ 'Jupiter' ] ],
  [ [ 'Venus' ], [ 'Jupiter' ] ],
  [ [ 'Sun' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Mercury' ] ],
  [ [ 'Sun' ], [ 'Mercury' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Venus' ], [ 'Mars' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Venus' ], [ 'Mars' ] ],
  [ [ 'Sun' ], [ 'Neptune' ] ],
  [ [ 'Sun' ], [ 'Neptune' ] ],
  [ [ 'Sun' ], [ 'Neptune' ] ],
  [ [ 'Venus' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Neptune' ] ],
  [ [ 'Venus' ], [ 'Uranus' ] ],
  [ [ 'Venus' ], [ 'Uranus' ] ],
  [ [ 'Venus' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Pluto' ] ],
  [ [ 'Sun' ], [ 'Pluto' ] ],
  [ [ 'Sun' ], [ 'Pluto' ] ],
  [ [ 'Sun' ], [ 'Pluto' ] ],
  [ [ 'Venus' ], [ 'Saturn' ] ],
  [ [ 'Venus' ], [ 'Saturn' ] ],
  [ [ 'Venus' ], [ 'Saturn' ] ],
  [ [ 'Venus' ], [ 'Saturn' ] ],
  [ [ 'Sun' ], [ 'Jupiter' ] ],
  [ [ 'Sun' ], [ 'Jupiter' ] ],
  [ [ 'Sun' ], [ 'Jupiter' ] ],
  [ [ 'Sun' ], [ 'Jupiter' ] ],
  [ [ 'Sun' ], [ 'Mars' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Mars' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Mars' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ],
  [ [ 'Sun' ], [ 'Mars' ] ],
  [ [ 'Mercury' ], [ 'Uranus' ] ]
]
[
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-sextil.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png',
  'https://horoscopes.astro-seek.com/seek-icons-astrologie/aspektarium/aspekt-midpointy-trigon.png'
]
fox4
```
