*this is a work in progress*

# A Modern Guide To Using SVG Icons
The writing has been on the wall for a couple of years by now: SVG is taking over the web. The SVG format solves a lot of problems, but does require learning some new techniques. The biggest stumbling block has always been spotty support from browsers, but those days are almost behind us (http://caniuse.com/#feat=svg). The #1 reason we should switch to SVG icons is to avoid browser-specific font-rendering issues without having to apply a bunch of hacky anti-aliasing and normalizing nonsense. SVG is not subject to any font-related hacks or effects. We don't have to worry about weird character mapping issues either. Yay! 

##Not your Fathers' SVG
For the uninitiated, SVG is a vector graphic format that supports multiple colors, gradients and can be styled through a bunch of CSS properties and SVG filter effects. The file format has been around since the early 2000s, but has only recently seen widespread support in browsers and authoring applications. Developers are pushing the limits of SVG further every day and with new techniques you can make SVGs do anything. You can stack them to make more complex icons and artwork. They can also be animated and dynamically controlled through javascript. These powers can be yours...IF you read this document AND follow some simple directions.

##The Mighty Quest
There are several established and popular methods for implementing SVG icons, however they all have shortcomings that render them useless for our purposes. Usually they involve generating PNG fallback files and other methods intended to patch-in support for less-capable browsers. While this can all be automated, it can bloat the project and it's increasingly unnecessary if you are targeting modern evergreen browsers like we do with Pinwheel&trade; and the new SDI Camps and Races F5 templates. Truth be told, there is a lot of old and conflicting information out there on the interwebs about the best way to use SVG icons which makes this ever more difficult. This document is a result of my findings, distilled into a (hopefully) helpful guide.

##So, whose Kung Fu is the best?
It really depends on your needs, but in our case I have combined the best parts of several techniques. Most of them lack an adequate method of applying CSS, if at all. One method involves setting the icon as a background image in CSS which means you can't style it and can't use it as a button or link. No thanks. Some actually *require* inline CSS which makes baby Jesus kittens cry. Other methods embed resources directly in the HTML instead of being stored in external files, thereby eliminating the benefits of caching. None of these methods are ideal because they require excessive work and resources. **What becomes clear after reviewing all these techniques is that we must treat SVGs as DOM objects in order to manipulate them with CSS and javascript.** Remember, if you try to use an ```.svg``` file as an image you **cannot** style it with external CSS. 

##The Secret Sauce
The secret is embedding ```<svg>``` tags inline, directly in the HTML with a ```<use>``` tag nested inside. This format allows us to select the desired icon sprite from the main ```spritemap.svg``` using unique IDs. To add external CSS suport you simply add an "icon-xxxxx" class on each ```<svg>```. You are free to build your styles in SASS if you wish.

##Efficiency
Using SVG icons this way reduces http requests and eliminates inefficiencies in our workflow. By moving all icons to an external file, we retain traditional caching abilities, while gaining the ability to style each instance of an icon seperately. Just what you wanted for Christmas, right? In some cases we may want to make several spritemaps, but the benefits remain the same. Whereas with icon fonts and PNG fallback methods, we serve and maintain possibly hundreds of asset files, with this method we serve a single maintainable, and easily cacheable XML file. Also, we no longer have to rely on a 3rd-party website to compile our icons into various formats. **Any developer** can add any icon to a project that employs this technique without even leaving their text editor!

##Compatibility
The techniques outlined in this document have been tested and verified working on the good browsers (Firefox 33, Chrome 39 - including Android, iOS 8 Safari, and latest desktop Safari). There are a couple minor tweaks to get 100% compatibility and those are noted in the source. Everything else just worked for me. Let me know if you run into any rendering issues.

##But What about Internet Explorer, You Say?
Well supposedly Internet Explorer now supports inline SVG...but you'll be *shocked* to learn that it doesn't support everything. In most cases, for modern browsers there is no longer a need to provide PNG fallback images. We can use a tiny javascript polyfill called **svg4everybody.js** that adds support for inline SVG with ```<use>``` tags. Soon, even that won't be necessary. 

## The Future
The next step is to automate as much of this as possible into a grunt or gulp task that will read a folder of icons, and build a properly formatted spritemap automatically using the icons filename. Unfortunately, current solutions do not account for the specific format we are using for this technique and will need to be modified. Once we get that going, this process will be even more efficient.

##TL;DR
Using SVGs as outlined here will greatly reduce http requests and completely eliminate font-rendering quirks. It also keeps our project clean by eliminating redundant files, and ultimately streamlines our workflow. Now any dev can copy an SVG icon path into the spritemap and start using it immediately with full CSS support! 

#The Demo

This quick and dirty demo will teach you how to add a single icon to a project. After you have cloned this project locally, you can open up the ```index.html``` in your editor and complete the demo tasks using the process outlined below. Essentially, you copy an icon to the project, copy its main ```<path>``` into the spritemap, then place a correctly formatted block in the HTML where the icon goes. Hit me up on chat or email if you have any questions or to complain about my awful documentation skills. :)

##Adding a New Icon to the Project: 
1. Add new ```.svg``` to **assets/icons/source** folder (**ALWAYS** keep a pristine version in the project)
2. Open new icon ```.svg``` in text editor and COPY the main shape (path d="..." - ignore empty paths)
3. Open ```'spritemap.svg'``` file in a text editor 
4. Duplicate the last ```<symbol>``` element at the bottom of the ```<svg>``` block
5. PASTE the path from step 2 inside this ```<symbol>```
7. Give the ```<symbol>``` a descriptive, unique id
6. PASTE the following block into the HTML where the icon should appear
```
  <svg class="icon-xxxxxx"> 
    <use xlink:href="assets/icons/spritemap.svg#xxxxxx"></use>
  </svg> (where ```xxxxxx``` matches the unique id of each symbol)
```
##Pros
1. You can control style with external CSS
2. Reduce http requests
3. Don't have to maintain and serve multiple font files for specific browsers
4. Any dev can easily add a new icon to the project at any time
5. Fewer files to maintain and no need to deal with .png fallback resources that just bloat the project
6. Eliminates font-rendering quirks including character mapping issues
7. Tiny javascript polyfill fixes Internet Explorer fails
8. Icons zoom with the rest of the content
9. Small file sizes that compress well

##Cons
1. Doesn't work in old browsers that we don't really care about.
2. There may be caching issues during development. If you are having trouble seeing icons in the browser, make sure caching is turned off.
3. Can't use font-related css attributes (this is also kind of a pro, but i needed more cons)

##Notes
* Decent overview of SVG techniques - http://css-tricks.com/using-svg/
* *svg4everybody.js* is a polyfill for using inline SVG in ie8-11 - https://github.com/jonathantneal/svg4everybody
* Each icon is contained in a ```<symbol>``` tag, which together make up the spritemap
* Why we use the ```<symbol>``` tag - http://css-tricks.com/svg-symbol-good-choice-icons/ 
* Source icons - https://github.com/google/material-design-icons or http://icomoon.io
* 48x48 default size, but they can be rendered any size
* Each icon is summoned with an ```<svg>``` tag containing a ```<use>``` tag that points to the id of the desired icon
* The id is only for selecting the icon from the svg spritesheet with the ```<use>``` tag
* The ```icon-xxxxxx``` class is for applying custom styling to a specific icon, or single instance of an icon
* SVG responds to a lot of standard CSS attributes, however there is a whole other world of SVG-specific CSS out there to explore - http://tutorials.jenkov.com/svg/svg-and-css.html
* Use "fill" instead of "color" to change an icon's color 
* Use "stroke: " and "stroke-width: " to outline the icon
* Have fun!

For help, please contact jared@sdicg.com.