# Using SVG icons the smart way

why we should use inline svg for icons
------------------------------
1. pro: you can control with css (http://css-tricks.com/using-svg/)
2. pro: reduce http requests to a single spritesheet
3. con: can't cache resources because they are embedded and therefore reloaded on every page load
4. get around this by using svg sprite and the svg <use> element to load each icon from the external spritesheet http://css-tricks.com/svg-use-external-source/
5. you retain the ability to cache svg sprites, and the ability the style with external css/sass
6. other methods require inline css inside the svg files, or the embedded svg object. having css embedded in the document all over the project will be ugly and bloat our markup.
7. pro: we don't have to manage and serve multiple font files for specific browsers
8. pro: any dev can easily add a new icon to the project at any time
9. pro: fewer files to maintain and no need to deal with .png fallback resources that just bloat the project
10. con: doesn't work with IE unless you use a polyfill like svg4everybody (https://github.com/jonathantneal/svg4everybody)


adding a new icon to the project: 
----------
1. add new .svg to /icon folder (always keep a pristine version in the project)
2. open new icon .svg in text editor, COPY the main shape (path d="..." - ignore empty paths)
3. open 'pg-icons.svg' file in text editor 
4. duplicate the empty <symbol> element right before the </svg> closing tag (ignore the <g> tags, we are substituting <symbol> for reasons outlined here - http://css-tricks.com/svg-symbol-good-choice-icons/)
5. PASTE the path you copied earlier and give it a unique id="" (leave the view-box alone)
6. paste this block in the html where you want to use the icon:

  <svg class="icon-IDNAME"> 
    <use xlink:href="assets/icons/pg-icons.svg#IDNAME"></use>
  </svg>
  (where 'IDNAME' matches the unique id of each symbol)

7. the unique id is only for selecting the icon from the svg spritesheet with the <use> tag in the HTML
8. the icon-IDNAME controls the styling of the icon in CSS/SASS
9. of all the ways to use svg this one makes the most sense because it removes a lot of the complexity and works around some of it's quirks of using svg for this purpose
10. summary: all icons are in a single svg file which limits http requests to one. we retain the ability to cache the spritesheet, and gain the ability the style with external css at the same time. 

additional notes and guidelines
-------------------------------
* https://github.com/jonathantneal/svg4everybody is a polyfill for using inline SVG in ie8-11.
* source icons come from http://icomoon.io or https://github.com/google/material-design-icons
* 48x48 default size, but can be made any size
* each icon is contained in a <symbol> tag, which together make up the spritesheet (pg-icons.svg)
* each icon is summoned with an <svg> tag containing a <use> tag that points to the id of the desired icon
* "icon-xxxxx" classes are for styling individual icons. they live in sass/_pg-icons.scss, and must be included on the <svg> tag or they won't work.
* SVG has special css attributes and behavior. to change an icon's color use "fill" instead of "color"
