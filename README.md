# :vibration_mode: SPONTIT :vibration_mode:
## Send push notifications without your own app. :punch:
Using the Spontit API and Spontit app/webapp, you can send your own push notifications programmatically to Android, iOS, and Desktop devices. You can send your own in less than 5 minutes. :sunglasses: :trophy: (Without touching Swift, Objective-C, Java, XCode, Android Studio, the App Store approval process... :dizzy_face:).


## TL;DR :running:

1) Sign up at <a href="https://www.spontit.com" target="_blank">spontit.com</a> (you might need to click "Take me to the Desktop version."). Note down your username. It should be displayed in the top right.
2) Get a secret key at <a href="https://www.spontit.com/secret_keys" target="_blank">spontit.com/secret_keys<a>. 
3) Get the iPhone app or Android app. Sign in and allow notifications.
4) `pip install spontit`
5) `from spontit import SpontitResource`
6) `resource = SpontitResource(my_username, my_secret_key)`
7) `response = resource.push("Hello!")`
8) You can customize the image of this notification on the website or iPhone app. You can push web content and can push to different topics (topic = subchannel). To push to others, have them follow your respective account (e.g. at <a href="https://spontit.com">spontit.com/my_username</a>) and/or topic. Currently, we only support topic creation on the iPhone app.
9) We are constantly working on expanding the functionality of Spontit. We GREATLY appreciate your input - feel free to <a href="https://github.com/joshwolff1/spontit_api/issues/new" target="_blank">add a feature request</a> on our Github. :smiley:

### About :information_source:

#### What are topics?
Every user, by definition, is a main channel. Each user can create a topic.

Topics are designed to act separately from your main channel. Users can follow topics without following your main channel. Users can follow your main channel without following your topics.

For example, my account user ID might be "elon_musk," but I might want to push about new SpaceX developments. I could push to the topic "spacex" and only those who follow the "spacex" topic would get pushed. 

When you create a topic, you only need to specify the display name. We create a topic ID from this display name. You then use this ID to programmatically push to the topic. To get the mapping of display name to ID, see "Send Your First Push Notification" below.

A notification pushed to a topic has an appearance defined independently from a notification that is pushed to a main channel (see "Push Notification UI Anatomy").

#### Creating Topics

Currently, creating topics is only supported through the GUI on the iOS <a href="https://itunes.apple.com/us/app/spontit/id1448318683" target="_blank">Spontit app</a>. We intend to expand this functionality to the website and the API. To create a topic, get the app, sign up, click the "+" in the top right, and then click "Your Topics", and then click the "+" in the top right.

Once you make a topic, you are NOT able to change its display name. This will likely NOT change for quite some time. Please keep this in mind when creating topics.


### Getting Started :white_check_mark:

#### Make an Account

First, go to <a href="https://www.spontit.com" target="_blank">spontit.com</a> or download the <a href="https://itunes.apple.com/us/app/spontit/id1448318683" target="_blank">Spontit app</a>.
Create an account and get your user ID. To see your user ID in the app, tap on the hamburger button. To see your user ID on the website, look at the top of the screen.

You can change your user ID at any time <a href="https://www.spontit.com/change_names" target="_blank">here</a>.

#### Generate a Secret Key

Once you have made an account, generate a secret key <a href="https://spontit.com/secret_keys">here</a>. You might have to re-authenticate.

#### Push Notification UI Anatomy

You can change your user ID, first name, and last name at any time <a href="https://www.spontit.com/change_names">here</a>.

<p align="center">
    <img src="https://github.com/joshwolff1/spontit_api/raw/master/images/main_channel_push.png" /> 
</p>

Above we see a push notification sent to a main channel. Here, "Josh Wolff" is the first and last name of the user. The call to action is the displayed text. The image shown is the personal profile picture of the user. (You can change your profile image on the homepage of the website or on the iPhone app in the sidebar.) If the user opens the notification, they can open a link attached, if any. If they have an iPhone, they can forward the notification and share it through several other mediums.

<p align="center">
    <img src="https://github.com/joshwolff1/spontit_api/raw/master/images/topic_push.png" /> 
</p>

Above we see a push notification to a topic. The user above could be managing this topic, but as you can see, it looks like its own account. "Dem 2020 Polls" is the display name, the non-bold text is the call to action, and the image is the image set for the topic. Currently, we only support setting images topic profile images on the iOS app. To set an image, go to "Your Topics" and click the camera icon beside the respective topic.
#### Send Your First Push Notification :calling:

The Spontit API currently only supports Python 3.6+.

`pip install spontit`

`from spontit import SpontitResource`

Construct the resource with your credentials.

`spontit_resource = SpontitResource("my_user_id", "my_secret_key")`

Push the notification to your main account. 

`response = spontit_resource.push("My First Push Notification")`

To specify a topic, specify a topic ID. The below code will send a push notification to the topic "mytopic," but it will not send a push notification to the main account. You can add more topics to the array. The topic must be created before pushing to it. To create a topic, see "Creating Topics" above.

`response = spontit_resource.push("My First Push Notification", to_topic_ids=["my_username/myowntopic"])`

You will need to get the topic ID. For example, you might specify a topic named "My Own Topic", but the ID you must use for pushing is "my_username/myowntopic". To get a mapping of topic IDs to display names, do the following:

`response = spontit_resource.get_topic_id_to_display_name_mapping()` 

#### Specify Content on Your Website

To link content from the Internet, do the following:

`response = spontit_resource.push("My First Push Notification", link="https://www.mywebsite.com", to_topic_ids=["mytopic"])`

### Limitations

#### Rate Limits

Each main channel and topic has an individual rate limit of 1 push per second. For example, you can push to your main account and two topics in the same second, but you cannot push 3 times to one topic in the same second.

If you exceed the rate limit, we will specify this in the response returned.

#### Note on Our Development Priorities

We prioritize development of the iOS application over the website. If at any time, we describe a feature and it does not seem to be on the website, it might only exist in the iOS application. Please email us at info {at} spontit {dot} io  so that we can clarify this to you and other developers. Please feel free to email us any feature requests as well!