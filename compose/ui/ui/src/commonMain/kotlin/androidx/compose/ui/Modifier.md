* == collection of `Modifier.Element` /
  * characteristics
    * ordered,
    * immutable
  * | Compose UI elements
    * decorate
    * add behavior
  * _Examples:_
    * _Example1:_ `androidx.compose.ui.samples.ModifierUsageSample`

* `Modifier` implementations
  * `then`
    * create a `Modifier` -- by starting from -- EXISTING modifiers / order is IMPORTANT
      * 👀modifier elements / appear first -> will be applied first 👀 
  * _Example:_ `androidx.compose.ui.samples.ModifierFactorySample`

* 👀Composables / 
  * accept a `Modifier` as a parameter | WHOLE UI component 👀
    * requirements
      * name the parameter -- as -- `modifier`
      * assign the parameter -- a -- default value
      * position as parameter
        * AFTER ALL required parameters (except for trailing lambda parameters)
        * FIRST optional parameter | parameter list 
          * == BEFORE any other parameters -- with -- default value
    * _Example:_ `androidx.compose.ui.samples.ModifierParameterSample`
  * accept modifiers | specific subcomponent `foo`
    * -> 
      * name the parameter `fooModifier`
      * follow the SAME guidelines ABOVE
      * ALL subcomponent modifiers -- should be -- 
        * grouped TOGETHER
        * follow the parent composable's modifier
    * _Example:_ `androidx.compose.ui.samples.SubcomponentModifierSample`