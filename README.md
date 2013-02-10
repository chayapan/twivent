# Twivent

Tweet reminders from Google Calendar events.

## Built using

* Playframework 2.1 Scala

## How to configure it using a configuration file

### Google Calendar API

You need a Google account and a [Google Calendar API access](https://code.google.com/apis/console).

Then, you'll get a P12 file containing your private key. In order to use this key through the config file, you have to extract it from the file using the following command line from your terminal/

```
$> openssl pkcs12 -in YOUR_P12_FILE -nodes -nocerts |  openssl pkcs8 -topk8 -nocrypt
```

The password to use is `notasecret`.

You'll get your private key which is something like:

```
-----BEGIN PRIVATE KEY-----
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
...
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxx==
-----END PRIVATE KEY-----
```

Then copy and paste it in the `conf/google-calendar.conf` file, in the `privateKey` property.

From the Google API Console, you also have to get your `accountId`, it looks like:

```
xxxxxxxxxxxx@developer.gserviceaccount.com
```

Copy and paste it in the `conf/google-calendar.conf` file, in the `accountId` property.

### Setting up your Google Calendar

You can get a list of your calendars using the following doc [https://developers.google.com/google-apps/calendar/v3/reference/calendarList/list](https://developers.google.com/google-apps/calendar/v3/reference/calendarList/list) by clicking the "Execute button".

Look for the id of your calendar, and then copy and paste it in the `conf/google-calendar.conf` file, in the `calendarId` property.

### To sum up

Your `conf/google-calendar.conf` file shoud look like:

```
google-calendar {
	accountId = "xxxxxxxxxxxx@developer.gserviceaccount.com"
	privateKey ="""
-----BEGIN PRIVATE KEY-----
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
...
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxx==
-----END PRIVATE KEY-----
	"""
	calendarId = "xxxxxxxxxxxx@group.calendar.google.com"
	applicationName = "YOUR_APP_NAME-YOUR_ORGANISATION_NAME/1.0"
}
```


## How to configure it for running under Heroku

Since you probably do not want your private data (private key, accountId etc.) to be store in your Git repo (I strongly discourage you to do so), and since you cannot upload files on Heroku without using a Git repo, the only way to configure your private data on Heroku is to use environment variables.

To do this, you have to use the `heroku config:add`command with the several env variables (take a look at the previous paragraph how to get them):

* google-calendar.accountId
* google-calendar.privateKey
* google-calendar.calendarId
* google-calendar.applicationName

For example:

```
$> heroku config:add google-calendar.accountId="xxxxxxxxxxxx@developer.gserviceaccount.com"

BEWARE: the private key must be on one line
$> heroku config:add google-calendar.privateKey="-----BEGIN PRIVATE KEY-----xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx..xxxx==-----END PRIVATE KEY-----"

$> heroku config:add google-calendar.calendarId="xxxxxxxxxxxx@group.calendar.google.com"

$> heroku config:add google-calendar.applicationName="YOUR_APP_NAME-YOUR_ORGANISATION_NAME/1.0"
```


## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-nc/3.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/3.0/deed.en_US">Creative Commons Attribution-NonCommercial 3.0 Unported License</a>.