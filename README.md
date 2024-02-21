 override fun onContent(post: Post) {
 
                findNavController().navigate(R.id.action_feedFragment_to_blankFragment,
                    Bundle().apply
                    { idArg = post.id })

            }


-------------------------------------------
object IdArg : ReadWriteProperty<Bundle, Long?> {


    override fun setValue(thisRef: Bundle, property: KProperty<*>, value: Long?) {
        if (value != null) {
            thisRef.putLong(property.name, value)
        }
    }

    override fun getValue(thisRef: Bundle, property: KProperty<*>): Long =
        thisRef.getLong(property.name)
}
-----------------------------------------------
class BlankFragment : Fragment() {

    private val service = Service()

    companion object {
        var Bundle.idArg: Long? by IdArg

    }........
    -----------------------------------------------
    //В NewPOstFragmente:
binding.bottomAppBar.setOnMenuItemClickListener { menuItem ->

            when (menuItem.itemId) {
                cancel -> {

                    findNavController().navigateUp()
                }

                else -> {
                    findNavController().navigateUp()
                }
            }
        }

        ------------------------------------
        анимация переходов в nav_main(В action фрагмента)
            app:enterAnim="@android:anim/fade_in" 
            app:exitAnim="@android:anim/cycle_interpolator"
            app:popEnterAnim="@android:anim/fade_in"
            app:popExitAnim="@android:anim/fade_out"
        
