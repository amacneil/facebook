Facebook Comments for EE 2.x
============================

Installation
------------

* Upload the `facebook_comments` folder to `system/expressionengine/third_party` on your web server
* In the admin CP, go to Add-Ons->Modules
* Click install on the facebook_comments module
* Jquery must be installed on the page(s) you are using this module on in order for all features to work properly.

Creating a Facebook application
-------------------------------

* Log in to Facebook and go to [http://facebook.com/developers](http://facebook.com/developers)
* If asked, allow Developers access to your information.
* Click "Create a New Application"
* Create an application with your chosen name
* Edit the application settings, and under Connect change the connect URL to the base URL of the site you are adding the module to (must end in / to be considered a directory, e.g. www.example.com/, not www.example.com)
* You may also need to set the post-authorization callback url. This is found under "Authorization" in the same control panel, and can be set to the same value as your Connect URL.

Setup
-----

* Take the Application ID (Not the API Key) and the Application Secret and enter them into the module control panel (Add-Ons -> Modules -> Click Facebook Comments).
* Change the default iFrame width to your desired setting. If left blank, this will assume 550px.
* Change the desired posts-per-page setting. This only applies to the iFrame version, and will default to 10.

XID Concept
-----------

* An XID is an identification used by Facebook to identify a particular piece of data. For the purposes of this module, it is a unique identifier for a set of comments. Any tags that reference a particular set of comments will need to have this parameter passed to them. The best choice for XID's is usually the entry_id of whichever piece of content the comments refer to, but you can use any combination of letters and numbers.

Template Tags
-------------

**{exp:facebook_comments:sdk}**

This adds the JS to load the Facebook SDK. Should be added in the footer just before closing the body tag. No JS will work without this!

**{exp:facebook_comments:logBox}**

Creates a login/logout button (the javascript SDK will determine which needs to be displayed)

**{exp:facebook_comments:getIframe}**

Creates a Facebook comment iframe. No templating required for this - loads directly from Facebook's server.

*Parameters*

* `xid` - REQUIRED. This is an identifier for this comment set. Recommended to set this to the EE entry's id (e.g. xid="{entry_id}")
* `width` - overrides the default width set in the CP
* `limit` - overrides the default number of posts per screen set in the CP

**{exp:facebook_comments:comments}**

Retrieves comments for templating (no iframe).

*Parameters*

* `xid` - REQUIRED. This is an identifier for this comment set. Recommended to set this to the EE entry's id (e.g. xid="{entry_id}")
* `limit` - Posts per page. Defaults to unlimited!
* `offset` - Page number, indexed from 0.
* `ajax` - Set this (to anything non-null) to enable ajax pagination.
* `nopagination` - Set this (to anything non-null) to disable pagination. Useful in conjunction with limit for displaying a limited number of "preview" comments.

*Variables*

Every variable returned by the Facebook API is exposed with the prefix "fb_". So, for instance, "text" would be accessible by `{fb_text}`. Also, any information returned about the user who made the comment is exposed with the prefix "fb_user_". So, the user's name would be `{fb_user_name}`.

Some useful examples:

* `{fb_text}` - Text of the comment
* `{fb_fromid}` -User who made the post's id
* `{fb_user_name}` - User's name
* `{fb_time}` - Time the comment was made.

`{fb_time}` can be formatted with a standard PHP time formatting string with the syntax `{fb_time="dateformat"}`. `{fb_time}` defaults to "d/m/y h:m A" (e.g. 8/5/10 5:00 PM).

Additionally, a helper tag has been made:

* `{fb_user_picture}` - A link to the user's picture

**{exp:facebook_comments:comment_form}**

Returns a form for adding comments to a given XID. If the user is not logged in, this will display a login request instead, and will update to a comment form on login.

*Parameters*

* `xid` - REQUIRED. This is an identifier for this comment set. Recommended to set this to the EE entry's id (e.g. xid="{entry_id}")

**{exp:facebook_comments:numComments}**

Retrieves the number of comments made for a given xid

*Parameters*

* `xid` - REQUIRED. This is an identifier for this comment set. Recommended to set this to the EE entry's id (e.g. xid="{entry_id}")
* `ajax` - If set (to anything non-null): Instead of using PHP's synchronous API to make this call, embed a bit of javascript to retrieve this client-side. HIGHLY recommended, as this will greatly speed up page processing time.
