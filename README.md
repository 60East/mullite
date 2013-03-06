mullite
=======

Review creator script for Atlassian Crucible.

mullite is a simple Python script that uses the Crucible and Fisheye REST APIs to automatically create, assign and update code reviews based on version control commits.

You'll need to provide configuration that includes the URL for your Fisheye / Crucible server and enumerates users and repositories to be covered by mullite.  Each repository has a list of users who are candidates for random review assignment.  You can also use special hashtags in review comments to control / skip review creation or update existing reviews.

### Configuration

mullite will look for a configuration file in its own directory named "mullite.conf".  The configuration file uses Python syntax and defines a handful of parameters used by the script.  Each section of the provided example configuration is described below.

First, the base URL under which all Fisheye and Crucible REST APIs are hosted.  In a standard deployment this should end with "rest-service":

```server_base = "http://example.com:8060/rest-service"```

Next, the credentials for an admin user.  This user will be recorded as the creator of reviews but not the owner or reviewer:
```
script_user = "crucibleadmin"
script_pass = "password"
```

The ```users``` map provides a mapping of full names to usernames.  This is used to help in mapping user information in commit descriptions to actual Crucible usernames:
```
users = {
   "Michael Bluth" : "michael",
   "George Michael Bluth" : "mr_manager",
   "Tobias Funke"  : "nevernude",
   "Gob Bluth"     : "illusionist",
}
```

The ```repos``` map defines both the set of repositories for which mullite will automatically create reviews and the reviewers who will be candidates for assignment in each repository if no reviewer is specified in a change's description:
```
repos = {
   "gobias-industries" : [ "nevernude", "illusionist" ],
   "banana-stand" : [ "michael", "mr_manager" ],
   "model-home" : [ "michael", "mr_manager", "nevernude" ],
}
```

Finally, ```history_window``` is an int that indicates how many changes into the past mullite should inspect each time it is executed.  
```
history_window = 50
```


### Commit Hashtags
mullite will inspect your change descriptions for various hashtag commands.  They can be included anywhere in the change comment.

#### #noreview
Tells mullite to skip this change entirely.

#### #review [review-id]
Tells mullite to append this change to the specified review rather than create a new review for it.  The review-id is the id string assigned by Crucible - it will be present in the URLs for an individual review and typically look something like CR-3.  And example of using this hashtag:

```fixes the defect mr_manager found in #review banana-stand-13 yesterday```

#### #reviewer [username-csv]
Tells mullite to assign a specific list of users to this review rather than selecting them at random from the users configured for this repository.  The username csv argument can be a single username or a space-free, comma-separated list of usernames.  For example:

```adding money to the banana stand #reviewer mr_manager,michael```

### Resources

####Python 2.6
    http://www.python.org/download/releases/2.6/

####Fisheye & Crucible REST APIs
    http://docs.atlassian.com/fisheye-crucible/latest/wadl/fisheye.html
    http://docs.atlassian.com/fisheye-crucible/latest/wadl/crucible.html
    http://docs.atlassian.com/fisheye-crucible/latest/wadl/fecru.html

### License

Licensed under The MIT License, check the mullite script for full details.


