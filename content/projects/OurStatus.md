# OurStatus
> A cross-platform social media app promoting productivity

---
**Web Platform**

<https://www.ourstat.us>

**Source Code**

<https://github.com/kevc528/OurStatus>

---

![OurStatus](/content/images/OurStatusLogo.jpg)

## About
This is a project I worked on during the summer of 2020 with a two of my friends from high school. The summer 
with COVID-19 was definitely different and this project was a great way to keep us busy! We found ourselves less 
productive with all this free quarantine time, which led to the idea of OurStatus - an app to improve 
productivity.

The goal of OurStatus is to encourage productivity by creating a social media experience. The idea stems from the 
human tendency to follow the crowd. If people around you are productive, you tend to want to be productive as 
well. So OurStatus combines task tracking with social media in order to make completing daily goals more 
interesting.

## Development
The development of this project is split into 3 smaller projects: a web app, an iOS app, and an Android app. 
Angular was used for the web app, Swift was used for the iOS app, and Java was used for the Android app. And all 
of these apps were connected using Firebase. My friends and I split these projects amongst each other, and I was 
responsible for the development of the web app.

### Firebase
In addition to developing the web app, I also designed the schema for the Cloud Firestore we used. Cloud 
Firestore is a NoSQL cloud database from Google's Firebase. Using a NoSQL database was nice because with the 
flexible schema, if we ever needed to make changes to the structure of data, it would be really simple. And using 
Cloud Firestore was very nice because of the ability to listen to a document for any changes, making live updates 
easy.

When I designed the schema of the database, I kept in mind the specific features and functionality in our app, 
and created the different collections based on this idea. So the main collections we had were users, tasks, 
comments, and friendship based on the main functionality of our app. To increase the performance, I also made 
sure to take advantage of using primary keys and creating composite indexes based on queries we'd have to use.

Other features I found useful were Firebase's authentication and cloud storage. Our app was able to use 
Firebase's authentication to securely store passwords, verify sign in credentials, and handle changing passwords. 
The cloud storage was useful for storing, updating, and retrieving profile pictures for each user.

### Angular App
For the Angular web app, I mainly relied on using the AngularFire library to interact with data. The AngularFire 
library is an official Angular library for Firebase that combines the native JavaScript Firebase SDK with RxJS. 
This library was really helpful because it was based on RxJS observables, making live updates to the app (i.e. feed, comments, etc.) very easy to implement in Angular.

##### Cookie Based Authentication
One important feature for my web app was the use of cookies to keep an authenticated session. This means that 
when a user logs in, a cookie is created so if they revisit the page they don't have to log in again. Obviously, 
that means the cookies have to be very secure and hard to forge, or else accounts could get hacked.

For this to work, the browser would store a cookie with a randomly generated ID having over a noniliion (10^30+) 
possiblities. This randomly generated session ID would be associated with the id of the signed in user, and this 
relationship was stored as a record in the database. This way, the client-side code has no control over that relationship. So whenever a user signs in, a cookie and database entry are created, and when the user signs out, the cookie and database entry are destroyed. Additionally, there are expiration times for the cookies that will invalidate that cookie and delete the database entry no matter what.

With this in place, the Angular app can look for a valid cookie first. And if it exists, it can automatically 
load the session state for the signed in user.

##### Session State
There are a few main pieces of information that drive the content for the user, like the username and user ID. 
Since this information is shared between all pages of the application, I decided to maintain this state by using 
NgRx, which is Angular's implementation of the redux pattern. I also decided to add a download link for the 
profile picture in the session state, so the sidebar doesn't have to wait to generate a link each time it's
loaded. This way, once the user logins in or a valid cookie is found, the user data has to only be loaded 
once and doesn't have to be passed between components.

##### Tasks, Feed, Profile
These features were fairly straightforward to implement and basically involved performing the CRUD operations on 
data in Firebase. What is important to note is that the rendering of these components are completely event-based, 
meaning the content live updates. This is due to the asynchronous and event-based nature of RxJS observables.

Something notable is that in the feed and comment section, the profile pictures for the users are lazy loaded, 
reducing the stress of fetching download links and rendering every friend's picture. Instead, pictures are 
rendered as they appear on the screen.

Additionally, users can view other user's profiles by either searching the username in the search component, 
clicking on users in the feed or comments, or clicking on users in the friends list. When visiting another 
user's profile, you can see the user's full name, username, and profile picture. And depending on the 
relationship, you can add friend, confirm/deny friend request, or remove friend.

##### Friend Recommendations
Since our app is a social media platform, we want the users to connect with as many other users as possible. To 
do this, I decided to implement friend recommendations in the web app. I wanted my recommendations to be 
accurate, but also have some aspect of randomness, so the recommendations wouldn't be the same each time.

I decided to approach my recommendations with the idea that if you are friends with another user and share many 
mutual friends with that user, it is likely that nonmutual friends with that user will also be people you know. 
To introduce randomness, I decided to first randomly select 10 friends, and then pick the 5 friends with the most 
mutuals. Then I created a weighting for these 5 based on the number of mutual friends they had with you, with 
more mutual friends getting a higher weight. Finally, I would randomly choose unique recommendations from each of 
the 5 friend's friend list, with the amount of recommendations chosen from that friend's list based on the weight 
assigned to that friend.

## Final Thoughts
Overall, working on this project with my friends was a very fun experience. We were able to have a lot of great 
technical discussions about different approaches while developing the platform. I thought it was a great use of 
my time during quarantine. I was able to build a useful applciation, while also furthering my development skills 
and learning about new technologies, like Firebase.