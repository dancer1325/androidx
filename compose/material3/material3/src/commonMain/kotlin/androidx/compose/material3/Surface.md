* Material surface
  * == central metaphor | material design
    * 👀== EACH surface exists | given elevation -> influences how👀 
      * it's -- visually related to -- OTHER surfaces
      * that surface -- is modified by -- tonal variance
  * is responsible for
    * Clipping
      * Surface clips its children -- to the -- shape / specified by `shape`
    * Borders
      * if `shape` has a border -> it will also be drawn
    * Background
      * Surface fills the shape / specified by `shape` -- with the -- `color`
      * if `color` is `ColorScheme.surface` -> a color overlay -- will be -- applied 
      * TODO:The color of the overlay depends on the [tonalElevation] of this Surface, and the [LocalAbsoluteTonalElevation] set by any parent  surfaces.
      * This ensures that a Surface never appears to have a lower elevation overlay than its ancestors, by summing the elevation of all previous Surfaces.
      * Content color: Surface uses [contentColor] to specify a preferred color for the content of  this surface - this is used by the [Text] and [Icon] components as a default color.
      * If no [contentColor] is set, this surface will try and match its background color to a color defined in the theme [ColorScheme], and return the corresponding content color. 
      * For example, if the [color] of this surface is [ColorScheme.surface], [contentColor] will be set to [ColorScheme.onSurface]
      * if `color` NOT included | theme palette -> `contentColor` == color | above this Surface
      * if you want to retrieve the Surface's content color -> use `LocalContentColor`
    *  Blocking touch propagation behind the surface ❓