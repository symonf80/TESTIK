 override fun onContent(post: Post) {
 
                findNavController().navigate(R.id.action_feedFragment_to_blankFragment,
                    Bundle().apply
                    { idArg = post.id.toString() })

            }
