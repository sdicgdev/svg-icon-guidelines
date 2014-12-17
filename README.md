*this is a work in progress*

# Making the Switch to SVG
The writing has been on the wall for a couple of years by now: SVG is taking over the web. The SVG format solves a lot of problems, but does require learning some new techniques. The #1 reason we should switch to SVG icons is so we can avoid any browser-specific font-rendering issues without having to apply a bunch of hacky anti-aliasing and normalizing nonsense. SVG is not subject to any font-related hacks or effects. We don't have to worry about weird character mapping issues either. Yay! The biggest stumbling block has always been spotty support in browsers (mostly IE), but those days are almost behind us.

##Not your Fathers' SVG
For the uninitiated, SVG is a vector graphic format that supports multiple colors, gradients and can be styled through a bunch of CSS properties and SVG filter effects. The format has been around for decades but has only recently seen widespread support. With modern techniques you can make them do anything. You can stack them to make more complex icons and artwork. They can also be animated and controlled through javascript. But before you get too excited, all these powers can be yours...IF you read the primer below AND follow some simple directions.

##Using SVG Icons the Smart Way &trade;
There are several established and popular methods for using SVG icons, however they all have shortcomings that need to be addressed if we want to use them. They usually involve generating .PNG fallback files and other methods intended to patch-in support for ancient browsers. While this can all be automated, it can unnecessarily bloat the project. Truth be told, there is a lot of old and conflicting information out there on the interwebs about the best way to use SVG icons. I have sifted through all the muck and absorbed as much relevant SVG knowledge as possible. After a mighty quest, I have returned with a new spell! 

##So many methods, which one is the best?
There are at least six ways to embed SVG in HTML. Most of them lack an adequate method of applying CSS, if at all. Some actually *require* inline CSS which makes baby Jesus kittens cry. Some methods eliminate caching behavior because the resources are embedded in the HTML instead of being stored in external files. None of these methods are ideal because they require more work to setup and more resources. TO be clear, we must treat SVGs as DOM objects in order to enjoy the benefits of manipulating them with CSS and javascript. Remember, if you try to use .svg files as images, whether by the <img> tag or configuring it as the background-image via CSS, you cannot style it with CSS. 

##The Secret Sauce
The secret is embedding <svg> tags inline, directly in the HTML with a <use> tag nested inside. This format allows us to select the desired icon sprite from the main spritemap.svg using unique IDs. To add external CSS suport you simply add an "icon-xxxxx" class on each <svg>. 

##Efficiency
Our ultimate goal is to reduce http requests and eliminate inefficiencies in our workflow. By moving all icons to an external file, we retain traditional caching abilities, while gaining the ability to style each instance of an icon seperately. Just what you wanted for Christmas, right? In some cases we may want to make several spritemaps, but the benefits remain the same. Whereas with icon fonts and PNG fallback methods, we serve and maintain possibly hundreds of asset files, with this method we serve a single maintainable, and easily cacheable XML file. Also, we no longer have to rely on a 3rd-party solution to compile our icons into various formats. **Any developer** can add any icon to a project that employs this technique without even leaving their text editor!

##Compatibility
The techniques outlined in this document have been tested and verified working on the good browsers (Firefox 33, Chrome 39 - including Android, iOS 8 Safari, and latest desktop Safari). There are a couple minor tweaks to get 100% compatibility and those are noted in the source. Everything else just worked for me. Let me know if you run into any rendering issues.

##But What about Internet Explorer, You Say?
Well supposedly Internet Explorer now supports inline SVG...but you'll be shocked to learn that it doesn't support every method. In most cases, for modern browsers there is no longer a need to provide PNG fallback images. We can use a tiny javascript polyfill called **svg4everybody.js** that adds support for inline SVG with <use> tags. Soon, even that won't be necessary. 

##Pros
1. You can control style with external CSS (http://css-tricks.com/using-svg/)
2. Reduce http requests
3. Don't have to maintain and serve multiple font files for specific browsers
4. Any dev can easily add a new icon to the project at any time
5. Fewer files to maintain and no need to deal with .png fallback resources that just bloat the project
6. Eliminates font-rendering quirks including character mapping issues
7. Tiny javascript polyfill fixes Internet Explorer fails
8. Icons zoom with the rest of the content.

##Cons
1. Doesn't work in old browsers that we don't really care about.
2. There may be caching issues during development. If you are having trouble seeing icons in the browser, make sure caching is turned off.
3. Can't use font-related css attributes (this is also kind of a pro, but i needed more cons)

##TL;DR
Using SVGs as outlined here will greatly reduce http requests and completely eliminate font-rendering quirks. It also keeps our project clean by eliminating redundant files, and ultimately streamlines our workflow. Now any dev can copy an SVG icon path into the spritemap and start using it immediately with full CSS support! 

#The Demo

This is intended to be a quick and dirty demo for adding a single icon to a project. After you have cloned this project locally, you can open up the index.html in your editor and complete the demo tasks using the process outlined below. Essentially, you copy an icon to the project, copy its path into the spritemap, then place a correctly formatted block of text in the markup. Hit me up on chat or email if you have any questions or to complain about my awful documentation skills. :)

##Adding a New Icon to the Project: 
1. Add new .svg to **assets/icons** folder (**ALWAYS** keep a pristine version in the project)
2. open new icon .svg in text editor and COPY the main shape (path d="..." - ignore empty paths)
3. Open 'spritemap.svg' file in text editor 
4. Duplicate the empty <symbol> element at the bottom of the <svg> block
5. PASTE the path you copied earlier and give it a unique id="" (leave the view-box alone and ignore the <g> tags, we are substituting <symbol> for reasons outlined here - http://css-tricks.com/svg-symbol-good-choice-icons/)
6. paste this block in the html where you want to use the icon:

  <svg class="icon-IDNAME"> 
    <use xlink:href="assets/icons/spritemap.svg#IDNAME"></use>
  </svg>
 
 (where 'IDNAME' matches the unique id of each symbol)

7. The id is only for selecting the icon from the svg spritesheet with the <use> tag in the HTML
8. The "icon-IDNAME" class is for applying custom styling to a specific icon

## The Future

The next step is to condense this process into a grunt or gulp task that will read a folder of icons, and build a properly formatted spritemap automatically. Unfortunately, current solutions do not account for the requisite format we need. That bit will make this even more efficient.

##Notes
* svg4everybody.js is a polyfill for using inline SVG in ie8-11 - https://github.com/jonathantneal/svg4everybody 
* source icons - https://github.com/google/material-design-icons or http://icomoon.io
* 48x48 default size, but can be any size
* each icon is contained in a <symbol> tag, which together make up the spritemap
* each icon is summoned with an <svg> tag containing a <use> tag that points to the id of the desired icon
* "icon-xxxxx" classes are for styling individual icons. they live in sass/_pg-icons.scss, and must be included on the <svg> tag or they won't work.
* SVG responds to a lot of standard CSS attributes, however there is a whole other world of SVG-specific CSS out there to explore - http://tutorials.jenkov.com/svg/svg-and-css.html
* Use "fill" instead of "color" to change an icon's color 
* Use "stroke: " to outline the whole symbol
* Use "stroke-width: " to control the width of the stroke
* Have fun!

For help, please contact jared@sdicg.com.