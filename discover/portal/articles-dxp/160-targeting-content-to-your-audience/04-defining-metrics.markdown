# Defining Metrics [](id=defining-metrics)

To provide your marketing team with real feedback from users, you can define the
user actions you want to track using *Metrics*. Metrics are used in reports to
measure the effectiveness of a campaign by tracking certain user actions.

Metrics filter the analytics data gathered by the Audience Targeting Analytics
engine to obtain the number of times a certain action was performed on a given
element or content by users and their user segments. 

Metrics are created by developers and deployed as extensions. Out of the box,
Audience Targeting includes metrics to track the most common user actions. These
metrics are described below.

### Content [](id=content)

Tracks the number of times content has been viewed. Use the content selector to
set the content to be tracked.

### Page [](id=page)

Tracks the number of times a selected page has been viewed. You can track views
on both both public or private pages.

### Form [](id=form)

Tracks how many users view a form, interact with it (i.e., type or select values
in the inputs), or submit it. If you select the *All* option from the *Event
type* field, the custom report shows the figures for the three events
simultaneously. You must also provide the form you want to track, which is
selectable from the *Form* metric.

### Link [](id=link)

Tracks how often links are clicked. This helps campaign administrators determine
if they're sufficiently visible or helpful.

Similar to forms, you must provide the ID of the link you want to track. If you
don't know it, you can inspect the HTML of the page where the link is and
extract this information.

### YouTube Videos [](id=youtube-videos)

Tracks how users interact with embedded YouTube videos. You must enter the
video's ID. You can extract this ID from the video URL as the value for the `v`
parameter. For instance, in the URL
`https://www.youtube.com/watch?v=2EPZxIC5ogU` the YouTube video ID is
`2EPZxIC5ogU`. Then select one of the available events, or *All* to track all of
them. For further reference on the meaning of these events, read the official
YouTube API documentation.

Notice that this option only works if the YouTube video is embedded as an
iframe. The iframe code is available from the YouTube video's *Share* &rarr;
*Embed* menu.

## Using Metrics [](id=using-metrics)

Suppose you want to run a campaign for an event that your company is hosting 
next month. You have created a main page for the event with a YouTube video and 
a *Register Now* banner. You also have a blog entry about the event displayed 
on several different pages and a Register page with the form to pay for 
registration. In this campaign, your goal is to get as many people to register
as possible, but there is other information you want to track to ensure that
everything is working as expected:

 - Visits to the main page of the event
 - Clicks to view the video
 - Number of users who watched the video until the end
 - Clicks on the Register Now banner
 - Views of the blog entry about the event
 - Views of the Register form
 - Number of users who started to fill out the Register form
 - Number of users who completed the registration

You can assign metrics to a campaign report, which is elaborated on in the next
section. To access the Metrics palette,

1.  Go to a pre-existing campaign.

2.  Select the *Reports* tab. 

3.  Add a custom report.

    The Metrics palette is accessible at the bottom of the *New Report* wizard.

You could drag and drop *metrics* from the palette to track all the actions
mentioned above. More types of metrics can be created by developers and deployed
as OSGi plugins. See the
[Tracking User Actions with Audience Targeting](/develop/tutorials/-/knowledge_base/7-1/tracking-user-actions-with-audience-targeting)
tutorial for details.

### Audience Targeting Analytics [](id=audience-targeting-analytics)

Metrics uses the *Audience Targeting Analytics* engine that can be configured 
per site or per @product@ installation. 

1.  To configure the analytics engine per Site, go to Site Administration and
    click *Configuration* &rarr; *Site Settings* &rarr; *Advanced* &rarr;
    *Audience Targeting Analytics*.

2.  To configure it per portal instance, Go to *Control Panel* &rarr;
    *Configuration* &rarr; *System Settings* &rarr; *Audience Targeting
    Analytics*.

The following analytics options are available:

- Anonymous Users (not available per site)
- Pages
- Content
- Forms
    - Form Views
    - Form Interactions
    - Form Submits
- Links
- YouTube Videos

Tracking all the actions of all your users can be a heavy load for your server. 
Therefore, it's best to disable tracking any actions about which you don't need
information. For example, by default, guest behavior analytics are tracked. This
stores a large amount of data to the database. If you're not interested in
tracking guest users,

1.  Disable the *Anonymous Users* selector.

2.  Click *Save*.

If you want to collect anonymous user data, but you are still mindful of
database resources, you can change the interval for how often anonymous data is
cleaned up. You can find the anonymous data storage setting under *Audience
Targeting Service*.

*  **Check Interval** and **Check Time Unit** define how often data cleanup
   takes place. If interval is set to *1* and unit is set to *Day* then the
   clean up task occurs once per day.

*  **Max Age** and **Max Age Time Unit** define how old the data must be to be
   deleted, so that data created immediately before the cleanup task is not
   immediately deleted. If the age was set to *10* and the unit set to *Hour*,
   any data older than 10 hours is removed, and any data less than 10 hours old
   is preserved until the next cleanup.

![Figure 1: You can manage anonymous data cleanup here.](../../images-dxp/anonymous-users-analytics.png)

Disabling analytics for certain entities means they aren't tracked. Carefully
manage analytics to optimize your Audience Targeting experience.

You can also store your analytics data in a separate database schema, which
allows for independent scalability. To separate the storage of analytics data
from Liferay's database schema,

1.  Navigate to the Control Panel &rarr; *Configuration* &rarr; *System
    Settings* &rarr; *Web Experience*

2.  Select *Audience Targeting Analytics Storage*.

3.  Fill out the external storage fields to point to your alternative database 
    schema.

![Figure 2: By filling out the external storage requirements, you configure your Audience Targeting analytics data to be stored in an alternative database schema.](../../images-dxp/alternative-analytics-db.png)

Once you've saved your external datasource configuration, you must restart the
Audience Targeting Analytics component.

1.  Navigate to the Control Panel &rarr; *Apps* &rarr; *App Manager* and select
    the *Liferay Audience Targeting* app suite.

2.  Select the *Options* (![Options](../../images-dxp/icon-app-options.png))
    button for the Analytics component and click *Deactivate*.

3.  Select the *Options* (![Options](../../images-dxp/icon-app-options.png))
    button for the Analytics component again and click *Activate*.
