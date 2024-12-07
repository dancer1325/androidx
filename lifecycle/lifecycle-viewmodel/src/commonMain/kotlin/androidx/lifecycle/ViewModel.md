* `ViewModel`
  * := class /
    * 👀-- responsible for --
      * preparing & managing the data for a 👀
        * `androidx.activity.ComponentActivity` or
        * `androidx.fragment.app.Fragment`
      * handles the communication 
        * `ComponentActivity` / `Fragment` -- with the -- rest of the application (== business logic classes)
        * BETWEEN `ComponentActivity` / `Fragment`
    * uses
      * acquire and keep the information / -- necessary for an -- `Activity` or a `Fragment` 
        * -> `Activity` or `Fragment` observe changes | `ViewModel`
        * ways to expose this information
          * `androidx.lifecycle.LiveData` or
          * Android Data Binding
    * -- should NOT
      * access your -- view hierarchy
      * hold a -- reference back to the `Activity` or `Fragment`
  * ALWAYS created -- in association with a -- scope (fragment or an activity) / 
    * retained AS LONG AS the scope is alive
      * _Example:_ if it is an Activity UNTIL it is finished
      * if its owner is destroyed for a configuration change (_Example:_ rotation) -> `ViewModel` will NOT be destroyed  
      * NEW owner instance -- just re-connects to the -- existing model
  * _Example:_ Activity standpoint
    ```
    class UserActivity : ComponentActivity {
       private val viewModel by viewModels<UserViewModel>()
       override fun onCreate(savedInstanceState: Bundle) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.user_activity_layout)
          viewModel.user.observe(this) { user: User ->
            // update ui.
          }
        requireViewById(R.id.button).setOnClickListener {
          viewModel.doAction()
        }
      }
    }
    
    class UserViewModel : ViewModel {
      private val userLiveData = MutableLiveData<User>()
      val user: LiveData<User> get() = userLiveData

      init {
        // trigger user load.
      }

      fun doAction() {
        // depending on the action, do necessary business logic calls and update the
        // userLiveData.
      } 
    }
    
    // Fragment -- acquires, by the SAME key as the Activity -- ViewModel 
    class MyFragment : Fragment {
      val viewModel by activityViewModels<UserViewModel>()
    }
    ```
