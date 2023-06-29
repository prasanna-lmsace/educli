Connect Moodle using webservice.
--------------------------------

With this plugin, administrators can seamlessly create users, courses, cohorts, enroll users in courses, and assign members to cohorts. This documentation will guide you through the usage and configuration of the connection with moodle.


### Setup:

1. Please install both local_educli and auth_educli plugins in your moodle.
2. Enable the educli authenication plugin in moodle auth methods.
    1. Go to '*Site Administration*' -> '*Plugins*' -> '*Authentication*' -> '*Manage authentication*'.
    2. In the "*Available authentication plugins*" table, find the plugin "**Educli authendication SSO**" and click the Eye icon to enable SSO.


### Generate site connection token.

1. Login as site administrator.
2. Go to 'Site administration' -> 'Plugins' -> 'Local Plugins' -> 'Educli'
3. Go to "General" tab, Enable the webservice by clicking the "Enable" button.
4. Enable the reset webprotocol by clicking the "Enable" button.
5. On here, go to 'Generate Token' tab. Select any of the user you want to assign webservices.

Note: Selected user must have assign role capability and other capabilities.

6. Finally, you can copy the "Site URL" and "Webservice Token" to your educli and SSO script.


## Configure SSO.

We added the script to make SSO with Moodle from your system.

1. Please replace the `$url` and `$token` with generated tokens.
2. Pass the data of any user to login in moodle,
3. Trigger any of the below mentioned function.

Plugin will create this user if that shared user doesn't exists in moodle.

In that SSO class, two functions are added to create SSO with moodle.

* `loggin_user_inmoodle` It creates a session for moodle in background, Using this method, can able to make the user login on moodle once they logged in your educli.
* `create_login_link` - This function generates a URL to login in moodle, When the user clicks the link it will redirect the user to moodle and creates a session in moodle.

Demo:

```
// Demo for SSO.
$url = 'http://YOURMOODLE.com';
$token = 'Token genenrated';
$userdata = ['email' => '29062023@test.com', 'username' => '29062023', 'firstname' => '29062023', 'lastname' => '29062023'];

(new MoodleSSO($url, $token))->loggin_user_inmoodle($userdata);

```

## Features:

In the local educli plugin, we added the methods to create users, courses and cohorts, and assign users to course and cohorts.

You can trigger the Moodle webserivce using the url http://YOURMOODLE.com/webservice/rest/server.php

Each webserive call should contain webserive token, name of the webservice function, and the data that webservice function arguments. Also you can pass the data return type with url its optional param (json or xml).

Demo webservice URL call:

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_enrol_users&moodlewsrestformat=json&enrolments[0][roleid]=5&enrolments[0][userid]=10&enrolments[0][courseid]=15&enrolments[1][roleid]=5&enrolments[1][userid]=10&enrolments[1][courseid]=14`

```
http://YOURMOODLE.com/webservice/rest/server.php
    ?wstoken=7374b92ccc20415e767a58980b003efb ---> Webservie token gennerated in educli moodle.
    &wsfunction=local_educli_enrol_users ---> Service function name to trigger
    &moodlewsrestformat=json ---> Data return type (JSON or XML)
    &enrolments[0][roleid]=5 --> data passed to service.
    &enrolments[0][userid]=10
    &enrolments[0][courseid]=15
    &enrolments[1][roleid]=5
    &enrolments[1][userid]=10
    &enrolments[1][courseid]=14

```


## Create Users:


This section contiains Webservice function, manadatory fields and optional fields data, Demo data and url strutures to create courses.


**WSfunciton**: `local_educli_create_users`

**Mandatory fields need to pass**: Users basic data

```
'createpassword' => 'True if password should be created and mailed to user.',
'username' =>  'Username policy is defined in Moodle security config.'
'auth' => 'Auth plugins include manual, ldap, etc (Default: manual)'
'password' => 'Plain text password consisting of any characters (If createpassword is true, then it should be empty)',
'firstname' => 'The first name(s) of the user,
'lastname' => 'The family name of the user',
'email' => 'A valid and unique email address',
```

**Optional Fields**

```
'createpassword' => 'True if password should be created and mailed to user.',
'username' =>  'Username policy is defined in Moodle security config.'
'auth' => 'Auth plugins include manual, ldap, etc'
'password' => 'Plain text password consisting of any characters',
'firstname' => 'The first name(s) of the user',
'lastname' => 'The family name of the user',
'email' => 'A valid and unique email address',
'maildisplay' =>  'Email visibility',
'city' => 'Home city of the user',
'country' => 'Home country code of the user, such as AU or CZ',
'timezone' =>  'Timezone code such as Australia/Perth, or 99 for default',
'description' => 'User profile description, no HTML',

'firstnamephonetic' => 'The first name(s) phonetically of the user',
'lastnamephonetic' => 'The family name phonetically of the user',
'middlename' => 'The middle name of the user',
'alternatename' =>  'The alternate name of the user'
'interests' => 'User interests (separated by commas)',
'idnumber' =>  'An arbitrary ID code number perhaps from the institution',
'institution' => 'institution',
'department' => 'department',
'phone1' => 'Phone 1',
'phone2' => 'Phone 2',
'address' =>'Postal address',

'lang' => 'Language code such as "en", must exist on server',
'calendartype' =>  'Calendar type such as "gregorian", must exist on server',
'theme' => 'Theme name such as "standard", must exist on server',
'mailformat' => 'Mail format code is 0 for plain text, 1 for HTML etc',

'customfields' => array(
        [
            'type'  => 'The name of the custom field',
            'value' => 'The value of the custom field'
        ],
        [
            'type'  => 'The name of the custom field',
            'value' => 'The value of the custom field'
        ]
),

'preferences' => array(
    [
        'type'  => 'The name of the preference',
        'value' => 'The value of the preference'
    ]
)
```

**Demo data structure**

```
'users' => array(
    0 => [
        'createpassword' => true,
        'username' => "29062023_002",
        'auth' => 'manual',
        'password' => "",
        'firstname' => "29062023_002",
        'lastname' => "LMSACE",
        'email' => "29062023_002@lmsace.com",
    ],
    1 => [
        'createpassword' => true,
        'username' => "29062023_003",
        'auth' => 'manual',
        'password' => "",
        'firstname' => "29062023_003",
        'lastname' => "LMSACE",
        'email' => "29062023_003@lmsace.com",
    ]
)
```

**Demo URL Structure:**

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_create_users&moodlewsrestformat=json&users[0][createpassword]=1&users[0][username]=29062023_002&users[0][auth]=manual&users[0][password]=&users[0][firstname]=29062023_002&users[0][lastname]=LMSACE&users[0][email]=29062023_002%40lmsace.com&users[1][createpassword]=1&users[1][username]=29062023_003&users[1][auth]=manual&users[1][password]=&users[1][firstname]=29062023_003&users[1][lastname]=LMSACE&users[1][email]=29062023_003%40lmsace.com`



## Create Courses:


This section contiains Webservice function, manadatory fields and optional fields data, Demo data and url strutures to create courses.


**WSfunciton**: `local_educli_create_courses`

**Mandatory data should passed**: Courses basic data

```
'fullname' => 'Course full name',
'shortname' => 'course short name',
'categoryid' => 'category id',
```

**Optional data**

```
'idnumber' => 'id number',
'summary' => 'summary',
'summaryformat' => 'summary',
'format' => 'course format: weeks, topics, social, site,..',
'showgrades' => '1 if grades are shown, otherwise 0',
'newsitems' => 'number of recent items appearing on the course page',
'startdate' => 'timestamp when the course start',
'enddate' => 'timestamp when the course end',
'maxbytes' => 'largest size of file that can be uploaded into the course',
'showreports' => 'are activity report shown (yes = 1, no =0)',
'visible' => '1: available to student, 0:not available',
'groupmode' => 'no group, separate, visible',
'groupmodeforce' => '1: yes, 0: no',
'defaultgroupingid' => 'default grouping id',
'enablecompletion' => 'Enabled, control via completion and activity settings. Disabled, not shown in activity settings.',
'completionnotify' => '1: yes 0: no',
'lang' => 'forced course language',
'forcetheme' => 'name of the force theme',
'courseformatoptions' => [
    array(
        'name' => 'course format option name',
        'value' => 'course format option value
    )
],
'customfields' => [
    array(
        'shortname'  => 'The shortname of the custom field',
        'value' => 'The value of the custom field',
    )
]

```

**Demo data structure**

```
'courses' => array(
    0 => [
        'fullname' => 'Course full name',
        'shortname' => 'course short name',
        'categoryid' => 'category id',
    ]
    ...// you can add Multiple courses here.
)

```

**Demo URL Structure**

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_create_courses&moodlewsrestformat=json&courses[0][fullname]=Demo+course+1+Educli&courses[0][shortname]=Demo+course+1+-+Educli&courses[0][categoryid]=1&courses[1][fullname]=Demo+course+2+Educli&courses[1][shortname]=Demo+course+2+Educli&courses[1][categoryid]=1`




## Enrol users into courses:


This section contiains Webservice function, manadatory fields and optional fields data, Demo data and url strutures to enrol users into courses.

**WSfunciton**: `local_educli_enrol_users`

**Mandatory fields should passed**: Enrolment basic data

```
'roleid' => 'Role to assign to the user',
'userid' => 'The user that is going to be enrolled',
'courseid' => 'The course to enrol the user role in',
```

**Optional fields**

```
'timestart' => 'Timestamp when the enrolment start',
'timeend' => 'Timestamp when the enrolment end',
'suspend' => 'set to 1 to suspend the enrolment',

```

**Demo data structure**

```
'enrolments' => array(
    0 => [
        'roleid' => "5", // Student role id
        'userid' => "10", // USER id
        'courseid' => "15", // Course id
    ],
    1 =>  [
        'roleid' => "5", // Student role id
        'userid' => "10", // USER id
        'courseid' => "14", // Course id
    ]
    ...// you can add Multiple courses here.
)

```

**Demo URL Structure**

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_enrol_users&moodlewsrestformat=json&enrolments[0][roleid]=5&enrolments[0][userid]=10&enrolments[0][courseid]=15&enrolments[1][roleid]=5&enrolments[1][userid]=10&enrolments[1][courseid]=14`



## Create cohorts:


This section contiains Webservice function, manadatory fields and optional fields data to create cohorts.

**WSfunciton**: `local_educli_create_cohorts`

**Mandatory fields should passed**: Cohort basic data

```

'categorytype' => array(
    array(
        'type' => 'The name of the field: id (numeric value of course category id) or idnumber (alphanumeric value of idnumber course category) or system (value ignored)',
        'value' => 'The value of the categorytype'
    )
),
'name' =>  'cohort name',
'idnumber' =>  'cohort idnumber',
// Optional feilds are goes here


```

**Optional fields**

```

'description' =>  'cohort description',
'descriptionformat' => 'description',
'visible' =>  'cohort visible',
'theme' => 'The cohort theme. The allowcohortthemes setting must be enabled on Moodle',

```

**Demo data structure**

```
'cohorts' => array(
    0 => [
        'categorytype' => [
            'type' => 'system',
            'value' => ''
        ],
        'name' => "Educli cohort 1",
        'idnumber' => "23062023_001",
    ],
    1 =>  [
        'categorytype' => [
            'type' => 'system',
            'value' => ''
        ],
        'name' => "Educli cohort 2",
        'idnumber' => "23062023_002",
    ],
    ...// you can add Multiple cohorts here.
)

```

**Demo URL Structure**

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_create_cohorts&moodlewsrestformat=json&cohorts[0][categorytype][type]=system&cohorts[0][categorytype][value]=&cohorts[0][name]=Educli+cohort+1&cohorts[0][idnumber]=23062023_001&cohorts[1][categorytype][type]=system&cohorts[1][categorytype][value]=&cohorts[1][name]=Educli+cohort+2&cohorts[1][idnumber]=23062023_002`



## Assign members to cohorts:


This section contiains Webservice function, manadatory fields and optional fields data to assign members to cohrots.

**WSfunciton**: `local_educli_add_cohort_members`

**Mandatory data should passed**: Cohort and user basic data

```
'cohorttype' => array(
    'type' => 'The name of the field: id (numeric value of cohortid) or idnumber (alphanumeric value of idnumber) ',
    'value' => 'The value of the cohort'
),
'usertype' => array(
    'type' => 'The name of the field: id (numeric value of id) or username (alphanumeric value of username) ',
    'value' => 'The value of the cohort'
)

```


**Demo data structure**

```
'members' => array(
    0 => [
        'cohorttype' => [
            'type' => 'idnumber',
            'value' => '23062023_002'
        ],
        'usertype' => [
            'type' => 'id', // id or username
            'value' => '10'
        ]
    ],
    1 => [
        'cohorttype' => [
            'type' => 'idnumber',
            'value' => '23062023_001'
        ],
        'usertype' => [
            'type' => 'id', // id or username
            'value' => '10'
        ]
    ],
    ...// you can add assign here.
)

```

**Demo URL Structure**

`http://YOURMOODLE.com/webservice/rest/server.php?wstoken=7374b92ccc20415e767a58980b003efb&wsfunction=local_educli_add_cohort_members&moodlewsrestformat=json&members[0][cohorttype][type]=idnumber&members[0][cohorttype][value]=23062023_002&members[0][usertype][type]=id&members[0][usertype][value]=10&members[1][cohorttype][type]=idnumber&members[1][cohorttype][value]=23062023_001&members[1][usertype][type]=id&members[1][usertype][value]=10`




