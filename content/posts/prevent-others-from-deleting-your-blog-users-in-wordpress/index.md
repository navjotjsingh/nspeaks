---
title: "Prevent Others from Deleting your Blog Users in Wordpress"
date: "2011-12-29"
categories: 
  - "wordpress"
summary: Protect your WordPress blog from getting deleted by another user.
---

When you have a multi-author blog having several administrators on your blog, it's important to have complete security setup on your blog. You may trust your co-administrators but still its better to have an edge with a bit of extra security measures. Now, administrators can delete any user from your blog. To prevent your co-administrators from doing this, you can follow either of the following two methods.

1. **Create Separate User Role** - The first method involves creating an additional User role having same powers as Administrator but without the _delete\_users_ capability. You can use [User Role Editor](https://wordpress.org/extend/plugins/user-role-editor/) plugin for this.

2. **Use the following code** in your theme's function.php or as a separate plugin.

    ```php
    add_action( 'delete_user', 'nspeaks_dont_delete_user' );
    function nspeaks_dont_delete_user( $id ) {
      $dont_delete_ids = array( 1,2,3,4 );
      if ( in_array( $id, $dont_delete_ids ) )
        wp_die( 'You cannot delete this user account.' );
    }
    ```

    Replace 1,2,3,4 with ids of the users you want to protect from deletion. The user will reach to the deletion screen but on final page he will encounter the error message given above. Thanks to [Justin Tadlock](https://justintadlock.com/) for [the code.](https://wpquestions.com/question/showLoggedIn/id/110)
