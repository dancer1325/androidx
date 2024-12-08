* `fun <T> mutableStateOf(
  value: T,
  policy: SnapshotMutationPolicy<T> = structuralEqualityPolicy()
  ): MutableState<T> = createSnapshotMutableState(value, policy)`
  * NEW `MutableState` / initialized with 1! `value`
    * `value` / reads & writes -- are observed by -- Compose

* `interface MutableState<T> : State<T> {}`
  * 1! value holder / reads & writes -- are observed by -- Compose
    * reads to the `value` property | execution of a `Composable` function 
    * writes
      * & changes -> recomposition of any subscribed `RecomposeScope`s  -- will be -- scheduled
      * / SAME value -> NO recompositions will be scheduled