# Rally Iteration Health

Measures health metrics for an iteration.   

This app can only be used at the leaf project level, meaning that data will not be calculated if the currently scoped project has open children.  

This app uses cumulative flow data for the selected project and iterations and can be used with any SaaS or On-Premise subscription that supports the 2.0 SDK.

Task Churn can be hidden via the App Settings.

![ScreenShot](/images/rally-iteration-health.png)
  
## Metric Definitions:  

### Scope Churn and Direction
Churn is a measure of the change in the iteration's scope.

Churn Direction (+/-) is an indicator of the general direction of scope change.  Churn is defined as a standard deviation, which is always zero or positive, so this added indicator provides an indication of whether scope tended to be added or removed

#### How it is calculated
Churn is defined as the standard deviation of the total scheduled into the sprint divided by the average daily total.

Churn Direction is determined by examining every day's change from the day before and adding or subtracting the delta to determine whether scope has been added more often than subtracted. (The first day of the iteration is excluded from this calculation.

### Percentage of Task Removal at Iteration End
Indicates when tasks have been added or removed on the last day of the iteration.  If a significant percentage of tasks are removed, it could be an indicator that the team is moving committed work items to another iteration.

#### How it is calculated
The number of estimated hours for the tasks scheduled in the iteration on the last day are subtracted from the total estimated hours of tasks scheduled on the next-to-last day, then divided by the next-to-last-day totals to create a percentage.  Note that this is calculated from the <b>estimates</b> of all the tasks, not the hours remaining to-do.

### Days  
The number of full days in the iteration (Excluding weekends)

### Percent Estimated
Represents the ratio of work items (stories and defects) that have estimates.

#### How it is calculated
Divide the number of work items (stories and defects) in the iteration that have a plan estimate that is not null by the total number of items in the iteration multiplied by 100. 
Stories that have a PlanEstimate = 0 (not null) will be counted as estimated.   
        
#### Coaching Tip
If there is a very high percentage or stories without estimates, other measures will not be meaningful.  This is really only useful for the beginning of an iteration, and perhaps for an iteration in early flight, but not for an iteration that has ended.  The idea is to catch this early in an iteration so other charts/graphs etc are useful for teams.  A good practice is to have a ready backlog as and entrance criteria to an iteration planning session, a ready backlog means three things, sized, ranked, and stories are elaborated sufficiently with acceptance criteria to enable conversation and confirmation during planning.

### Last Day Acceptance Ratio
Indicates whether teams met their commitment, assuming work items have not been removed from the iteration. 

#### How it is calculated
Divide the plan estimates of the work items in the iteration that were accepted on the last day of the iteration by the total plan estimate of all work items in the iteration.  If analysis type is set to 'counts', the calculation is based on the number of work items, not the plan estimate of the work items.

### Average Daily In-Progress Percentage
This is an indication of how much work is in progress (WIP).  It is the ratio of the average of the work items in the in-Progress state on a daily basis. 

#### How it is calculated
Divide the plan estimate of all the work items in the 'in-progress' state by the total plan estimate of the work items in the iteration, divided by the number of days.  If the iteration is in-flight, we'll divide by the number of days so far.   If analysis type is set to counts, the calculation is based on the count of the work items, not the plan estimate of the work items.

If there are no plan estimate totals for an item on one day, the in progress ratio is calculated as 0 for that day.  This means that if there are no work items in a sprint for the first 5 days of the sprint, the average of in-progress items will include those days where there were no items in the sprint.          
        
#### Coaching Tip
A high percentage here would mean that there is a high degree of daily WIP on average.  Keeping WIP small, reduces context switching and helps team focus on the most important items to reach acceptance.
    
### Last Day Completion Ratio
Represents the ratio of work completed by iteration end.  A low percentage might imply that there is work planned into an iteration that was left in a schedule state lower than completed.

#### How it is calculated
Divide the plan estimates of the work items in the iteration that are in a schedule state that is Completed or higher at the end of the last day of the iteration by the total plan estimate of all work items in the iteration. If analysis type is set to 'counts', the calculation is based on the count of the work items, not the plan estimate of the work items.

### Percentage of Task Removal At the end of the sprint
This shows the percentage of task estimate that is removed from the 2nd to last day of the sprint to the last day of the sprint.  

No Data means that there were never any estimated tasks in the sprint.  A task churn of 0% could mean that the task removal didn't change on the last 2 days, or that tasks were removed prior to the last day of the sprint.  

### Velocity Variance
This shows the % variance of the velocity from the average of the past 3 sprints. If there is only data from less than that, then that data will be used.  Otherwise the velocity variance will show as No Data.  The velocities used in this calculation is the current velocity, which is calculated by adding all stories associated with the iteration as of the current date.

### Cycle Time
This shows the average cycle time of stories in the sprint. This can be configured from the App Settings... menu.  There are up to three options:

  * **No** : Do not show cycle time
  * **In Progress to Accepted** : Calculated by counting the hours from the InProgressDate field to the AcceptedDate field
  * **In Progress to Completed** : (Only available/selectable in SAAS) Calculated using lookback to count the hours from first transition to
      In-Progress (or higher) to the last transition to Completed (or higher).  Does NOT consider if moves left of In-Progress after first hit.


### Show Local Date
Check this box to show sprint beginning and end date in the browser's local time.  Otherwise show it in the workspace's default.  
When the workspace is in a timezone to the west of the user's timezone, local time can be a day later than would be shown in workspace
timezone.

### Show Say:Do Ratio
This box should only be available when lookback is available.  Check this box to calculate the say/do ratio: The app will 
review the list of items assigned to the iteration on the first day and the items assigned on the last day.  Those items that
were there on the first day and are still in the iteration & have been accepted will be divided by the items that were on the 
first day to determine a percentage.  The percentage will always be 100% or less because count and plan estimate are used as 
set from the beginning of the sprint.


## Building this code

### First Load

If you've just downloaded this from github and you want to do development, 
you're going to need to have these installed:

 * node.js
 * grunt-cli
 * grunt-init
    
 Since you're getting this from github, we assume you have the command line
 version of git also installed.  If not, go get git.

 If you have those three installed, just type this in the root directory here
 to get set up to develop:

 npm install

### Structure

 * src/javascript:  All the JS files saved here will be compiled into the 
    target html file
 * src/style: All of the stylesheets saved here will be compiled into the 
    target html file
 * test/fast: Fast jasmine tests go here.  There should also be a helper 
    file that is loaded first for creating mocks and doing other shortcuts 
    (fastHelper.js) **Tests should be in a file named <something>-spec.js** 
 * test/slow: Slow jasmine tests go here.  There should also be a helper
    file that is loaded first for creating mocks and doing other shortcuts 
    (slowHelper.js) **Tests should be in a file named <something>-spec.js**
 * templates: This is where templates that are used to create the production
    and debug html files live.  The advantage of using these templates is that
    you can configure the behavior of the html around the JS.
 * config.json: This file contains the configuration settings necessary to
   create the debug and production html files.  
 * package.json: This file lists the dependencies for grunt
 * auth.json: This file should NOT be checked in.  Create this to create a
   debug version of the app, to run the slow test specs and/or to use grunt to
   install the app in your test environment.  It should look like:
   {
     "username":"you@company.com",
     "password":"secret",
     "server": "https://rally1.rallydev.com"
   }
                                                                            
### Usage of the grunt file

#### Tasks
                                                                                
##### grunt debug
  Use grunt debug to create the debug html file.  You only need to 
  run this when you have added new files to the src directories.
    
##### grunt build

Use grunt build to create the production html file.  We still have to copy the html file to a panel to test.

##### grunt test-fast

Use grunt test-fast to run the Jasmine tests in the fast 
directory.  Typically, the tests in the fast 
directory are more pure unit tests and do not need to connect to Rally.

##### grunt test-slow

Use grunt test-slow to run the Jasmine tests in the slow 
directory.  Typically, the tests in the slow
directory are more like integration tests in that they 
require connecting to Rally and interacting with
data.

##### grunt deploy

Use grunt deploy to build the deploy file and then install it into a new page/app in Rally.  It will create the page on the Home tab and then add a custom html app to the page.  The page will be named using the "name" key in the config.json file (with an asterisk prepended).

To use this task, you must create an auth.json file that contains the following keys:
{
        "username": "fred@fred.com",
        "password": "fredfredfred",
        "server": "https://us1.rallydev.com"
}

(Use your username and password, of course.)  NOTE: not sure why yet, but this task does not work against the demo environments.  Also, .gitignore is configured so that this file does not get committed.  Do not commit this file with a password in it!

When the first install is complete, the script will add the ObjectIDs of the page and panel to the auth.json file, so that it looks like this:

{
        "username": "fred@fred.com",
        "password": "fredfredfred",
        "server": "https://us1.rallydev.com",
        "pageOid": "52339218186",
        "panelOid": 52339218188
}

On subsequent installs, the script will write to this same page/app. Remove the
pageOid and panelOid lines to install in a new place.  CAUTION:  Currently, error checking is not enabled, so it will fail silently.

##### grunt watch

Run this to watch files (js and css).  When a file is saved, the task will automatically build and deploy as shown in the deploy section above.
