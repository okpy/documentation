Autograder Documentation
=====
The Server Side Autograder executes your grading code in a sandboxed [Docker](https://docker.com) container on a remote server.
> [Login >](https://autograder.cs61a.org/login)

The autograder supports <b>any language or grading setup</b> that can run in a virtual machine.

Jobs are submitted to the autograder using a REST API.

## Request Access

If you are an computer science educator, you can request access by sending an email to sumukh@berkeley.edu with details about your use case.

> [Request Access >](mailto://sumukh@berkeley.edu?subject=OK%20Autograder)

# Setup

All requests to the autograder require an assignment specific access key. Login to the autograder dashboard to obtain this key.

## Creating an assignment
Create an assignment on the autograder dashboard. To create an assignment, go to the menu "Assignment" > "Create"

> <h4>Example Config</h4>

> * Name:  "Yelp Maps"
> * Zip File URL: "http://nifty.stanford.edu/2016/hou-zhang-denero-yelp-maps/maps_deploy.zip"
> * Email Student: ✓
> * Submission Wait Time: "43200" (12 hours)
> * Grading script:
> ```
> #!/bin/sh
> python3 ok -v; # Your grading command
> # ./custom.sh; (or call your own script in the zip)
> rm -rf ./*; # please cleanup your files```


* Name: The assignment name
* Zip File URL: Supporting files required to grade an assignment (helper files and assets). When grading, this is unzipped into the grading folder.
* Email Student: If the student's email is provided and this box is checked, the output will be emailed to the sutdent
* Submission Wait Time: The cooldown period (in seconds) that a student must wait before recieving another email notification from the autograder. Default: 30 seconds.
* Grading Script: A bash script that specifies how to grade. The autograder captures any text this scripts prints to STDOUT. It ignores STDERR.

### Obtaining the access key

Once the assignment is created, the ID for the assignment will be displayed on the dashboard.
> `ID: a4f023f90l1d32123kj84l9`

This ID is the assignment access key for requests to the autograder


### Cooldown Periods

Some courses may want to limit the frequency of submissions from students to the autograder. The cooldown period setting accomplishes this. The cooldown period is unique to the submitter.

> <h4>Cooldown Email</h4>

> - Subject: cs61a-su16 Autograder Limit Reached
- From: no-reply@okpy.org
- Body: Autograder Limit: Try Again in 26 seconds from now

If a student has already recieved a notification from the autograder, they will recieve an email from the autograder that tells them how long they have to wait.

### Email Notifications

If email notifications are turned on, emails are provided, and the submitter is not in a cooldown period, an email will be sent with the output of the grading script.

If you want to make sure that the tests are not visible to submitters, ensure that your grading script does not leak the tests.
Be sure to handle


# Grading Environment

### Job Queue
The continous autograder responds in a first come first serve basis. This means that you can submit multiple jobs and the autograder will handle them in order. The amount of waiting time for a specific job is not guaranteed, but is usually in the order of a few seconds under normal load.

### OS & Libraries

By default, assignments are run in a container with many common dependices installed. You can view this container in [Docker Hub](https://hub.docker.com/r/cs61a/enviroment/)

> Ubuntu Linux 14.04 64 bit
> Python 3.4 with many common libraries
> numpy, scipy
> Network access disabled by default.

To install other libraries, notify us.

If you need to run under a custom docker image, contact us.

### Timeout

The timeout for a submission is 300 seconds. If a job does not terminate in 300 seconds, it will timeout and no notification will be sent. Be sure to handle this in your grading script. A popular tool is the [`timeout` command](http://man7.org/linux/man-pages/man1/timeout.1.html).

> In your grading script: `timeout 60 ./grade.sh`


# API

The following sections describes the API methods for the autograder.

## Email Grading

#### Endpoint

`POST https://autograder.cs61a.org/api/file/grade/continous`


#### Arguments
>><h4> Example Request </h4>
```python
data = {
        'assignment': 'a4f023f90l1d32123kj84l9',
        'file_contents': {
        	'maps.py': 'print("hello world")\nprint("bye")'
        	'reccomend.py': 'print("reccomend")'
        },
        'submitter': 'sumukh@okpy.org',
        'emails': ['sumukh@okpy.org', 'partner@example.com']
}
headers = {'Content-type': 'application/json', 'Accept': 'text/plain'}
r = requests.post('https://autograder.cs61a.org/api/file/grade/continous',
                  data=json.dumps(data), headers=headers, timeout=10)
success = r.status_code == requests.codes.ok
```

| Arugment    | Type    | Description |
|-------------|------------------------------|
| assignment  | string:required | The ID of the assignment from the dashboard |
| file_contents| dictionary:required |A dictionary that maps filename to the contents. |
| submitter    | string:required | The email of the submitter. Used for cooldown period and for sending emails. |
| emails       | list of string:optional | List of emails that should recieve the autograder email (for groups) |
| testing      | boolean:optional | If true, it will not be graded. |

#### Response
>> <h4> Example Response </h4>
```python
{ 'ok': 'ok' }```

If the arguments are invalid, the server will respond with a HTTP error and an error message.

Otherwise the response will be a 200 with the following contents:

| Arugment    | Type    | Description        |
|-------------|------------------------------|
| ok          | string  | The string "ok" if succesful   |


### Email
The autograder will generate an email to send to the requested address once the job has completed. The contents will be the output of your grading script to STDOUT.

```
Subject: cs61a-su16 Autograder Repsonse
From: no-reply@okpy.org
To: sumukh@okpy.org, partner@example.com
Body:
Autograder Response:
hello world
bye
reccomend
Point breakdown
    Question 0: 0.0/0
    Question 1: 0.0/2
    Question 2: 0.0/1
```

## Webhook Grading

#### Endpoint

`POST https://autograder.cs61a.org/api/file/grade/webhook`

#### Arguments
>><h4> Example Request </h4>
```python
{
        'assignment': 'a4f023f90l1d32123kj84l9',
        'file_contents': {
        	'maps.py': 'print("hello world")\nprint("bye")'
        	'reccomend.py': 'print("reccomend")'
        },
        'webhook': 'https://example.com/sumbmission/info/8sj2',
}
```

| Arugment    | Type    | Description |
|-------------|------------------------------|
| assignment  | string:required | The ID of the assignment from the dashboard |
| file_contents| dictionary:required |A dictionary that maps filename to the contents. |
| webhook    | string:required | The URL to POST the output back to. |

#### Response
>> <h4> Example Response </h4>
```python
{ 'ok': 'ok' }```

If the arguments are invalid, the server will respond with a HTTP error and an error message.

Otherwise the response will be a 200 with the following contents:

| Arugment    | Type    | Description        |
|-------------|------------------------------|
| ok          | string  | The string "ok" if succesful   |


### Webhook
>><h4> Example Webhook Response (from the autograder) </h4>
```python
data = {'output': 'hello world\nbye\nreccomend\nPoint Breakdown...'}
headers = {'Content-type': 'application/json', 'Accept': 'text/plain'}
r = requests.post("https://example.com/sumbmission/info/8sj2",
                  data=json.dumps(data), headers=headers, timeout=10)
```


The autograder will generate an POST request to send to the webhook URL once the job has completed. The contents will be the output of your grading script to STDOUT.

| Field Name  | Type    | Description |
|-------------|------------------------------|
| output  | string | The contents of the output |

The autograder will only attempt to POST once. It may retry if the request fails (a non `200` response code), but the retry is not guaranteed.


# OK Integration

The OK Server uses the autograder to grade assignments. If you are using the OK server, the API has already been integrated into the server. The OK integration uses private API methods to work. Those methods are not detailed here.

## CS61A Assignments

The following instructions are useful if you are grading CS61A Assignments. If you are grading iPython Notebooks - there are separate instructions (not posted here)

### Upload zip
>><h4> Example Grading Script </h4>
>>```
mv lab00/* .;
timeout 60 python3 ok --local --no-update --score --score-out results.txt;
python3 parse_output.py results.txt lab 08/29/2016 --full-cutoff 1;
rm -rf ./*
```

* Download zip:
`curl http://cs61a.org/lab/lab00/lab00.zip > lab00.zip`
`unzip lab00.zip`
* Delete starter files (i.e. any files that OK submits)
`rm lab00/lab00.py`
* Put the grading script in (parse_output.py)
`cp parse_output.py lab00/parse_output.py`

* Edit the .ok file to add the "scoring" protocol: `vim lab00/lab00.ok`
* Rezip it: `zip lab00-grading.zip -r lab00/`
* Upload the zip to https://autograder.cs61a.org/admin/gradingfileadmin/
Create assignment
   * Go to https://autograder.cs61a.org/admin/assignment/ and create a new assignment
   * Name: Lab 0
   * Grading script: similar to the example above.
    You need to tweak assignment type (lab/hw/quiz/proj) and due date
    For lab and hw, `--full-cutoff` is the number of points needed for full credit.
    Additionally, for hw, `--partial-cutoff` is the number of points needed for partial credit.
* Zip file URL <secret>/lab00-grading.zip
  * The URL convention should be clear from examples
  * Replace filename with file name of what you uploaded.
  * Do not check "EMAIL STUDENTS" - if you do emails will go out to students
  * Submission wait time: 1 (second) - only useful if you have continuous auto grading enabled.


### Autograde from OK
* Click on an assignment on OK. Click "Autograde"
* Copy-paste your OK access token. Use a fresh one so that it doesn’t expire while the assignment is being graded.
* Copy-paste the assignment ID from the autograder (e.g. 57d1e....).
* You don't have to enable "Backup Autopromotion". The most recent backups will be graded anyway if the student doesn't have a submission.
* Hit "Queue on Autograder"

### Monitor
There isn't a good solution right now. Come back in half an hour and check the score export. For debugging issues, ping Sumukh.

* The submissions tab shows that your assignments

# Custom Integrations

It may be possible to integrate the autograder into your existing course workflow for submission (Git, Email, Upload etc). Contact us to get information.
