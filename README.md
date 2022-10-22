Original App Design Project - README Template
===
 
# Visual Fitness
 
## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)
 
## Overview
### Description
 
This app will give a suggestion of food, exercise, treatment, nearby resturents based on the body part user chose. User can view it in video, gif and article format, which will make it very interactive and user friendly.
 
### App Evaluation
[Evaluation of your app across the following attributes]
- **Category:**
- **Mobile:**
- **Story:**
- **Market:**
- **Habit:**
- **Scope:**
 
## Product Spec
 
### 1. User Stories (Required and Optional)
 
**Required Must-have Stories**
 
* User can able to see a 3d human skeleton and can click on it to get the result. 
* Log in/ sign up
* Detailed information
* Can be able to save the articles or videos.
 
**Optional Nice-to-have Stories**
 
 
 
### 2. Screen Archetypes
 
* Launch screen
  * Will show the 3d human skeleton body
 * Log in/ Sign up screen
  * User can create an account to save the results.
 
### 3. Navigation
 
**Tab Navigation** (Tab to Screen)
 
* Body
* User
 
 
**Flow Navigation** (Screen to Screen)
 
* Launch screen
  * Log in/ Sign up screen
 
* profile
  * Saved data
  * Log out
 
## Wireframes
[Add picture of your hand sketched wireframes in this section]
<img src="https://drive.google.com/uc?export=view&id=1vd1scTRGYcdYA1rK6qNVDxhnozjXq4KW" width=600>
 
### [BONUS] Digital Wireframes & Mockups
 
### [BONUS] Interactive Prototype
 
## Schema
[This section will be completed in Unit 9]
### Models
[Add table of models]
**USER**
| Property       | Type      | Description                 |
| -------------- | --------- | --------------------------- |
| Username       |  String   | Unique name for user        |
| Password       |  String   | Password for user, hashed   |
| Email          |  String   | Email of the user, url      |
| Image          |  Image    | Image                       |
| Like Count     |  Integer  | # times user clicks like    |

**SAVED**
| Property       | Type      | Description                 |
| -------------- | --------- | --------------------------- |
| author         |  String   | Owner of the list           |
| Image          |  Image    | The saved image             |
| description    |  String   | The description for image   |

### Networking
- [Add list of network requests by screen ]
- Lauch Screen 
    - (Create/POST) Create a new team account
        ```
        let user = PFUser()
        user.username = usernameField.text
        user.password = passwordField.text
        
        user.signUpInBackground { success, Error in
            if success {
            self.performSegue(withIdentifier: "", sender: nil)
            }
            
            else{
                print("Error: \(Error?.localizedDescription)")
            }
        } 
        ```
    - (Read/GET) logging in an existing user
        ```
        let username = usernameField.text!
        let password = passwordField.text!
        PFUser.logInWithUsername(inBackground: username, password: password) { user, error in
            if user != nil {
                self.performSegue(withIdentifier: "", sender: nil)
            }
            else {
                print("error: \(error?.localizedDescription)")
            }
        }
        ```
- Home Screen 
    - (Read/GET) retrieve information on body part in search bar 
    - (Read/Get) Query information available for display for user 
        ```
        let query = PFQuery(className:"User")
        query.whereKey("user", equalTo: currentUser)
        query.order(byDescending: "createdAt")
        query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
                print(error.localizedDescription)
            } else if let posts = posts {
                print("Successfully retrieved \(posts.count) posts.")
            // TODO: Do something with result...
            }
        }
        ```
    - (Read/GET) query for random body part 
- Yelp Screen 
    - (Read/GET) Get articles related to body part
    - (Read/GET) Get video/gif of related body part 
    - (Read/GET) Get recommendations based of related body part 
- Video Screen 
    - (Read/GET) Get video for body part 
    - (Read/GET) Get article for body part 
- Saved Screen
    - (Create/POST) add new article/image/food/names to the Saved table
        ```
        // Create the Saved table
        let saved = PFObject(className: "Saved")
        saved["description"] = text
        saved["image"] = selectedSaved
        saved["author"] = PFUser.current()!

        selectedSaved.add(saved, forKey: "saves")
        selectedSaved
            .saveInBackground { success, error in
            if success {
                print("successfully updated")
            }
            else {
                print("failed to update")
            }
        }
        ```
    - (Read/GET) query for the contents in the Saved table 
        ```
        let cell = tableView.dequeueReusableCell(withIdentifier: "CommentCell") as! CommentCell
        let comment = comments[indexPath.row - 1]
        cell.commentLabel.text = comment["text"] as? String
        let user = comment["author"] as! PFUser
        cell.nameLabel.text = user.username
        return cell
        ```
    - (Delete) Remove an item from the Saved table
- [Create basic snippets for each Parse network request]
- [OPTIONAL: List endpoints if using existing API such as Yelp]

