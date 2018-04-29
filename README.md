# MeetupPS

PowerShell module to interact with the Meetup.com API

![image-center](/media/meetupAPI3.png)

## Contribute

Contributions are welcome by using pull request and issues.

## Table of Contents

* [Install the module](#Install)
* [Configure connection](#Configure)
* [Authentication](#Authentication)
* [Get Meetup Group Information](#GetGroupInfo)
* [Get Meetup Group's events Information](#GetEventInfo)
  * [Get upcoming events](#GetupcomingEventInfo)
  * [Get past events](#GetpastEventInfo)
* [Create Meetup Event](#CreateEvent)
* [API Permission scopes](#APIPermissionScopes)
* [Resources](#Resources)

<a name="Install"/>

## Install

Install the module from the PowerShell Gallery.

```powershell
Install-Module -Name MeetupPS
```

<a name="Configure"/>

## Configure connection

Follow the following steps to request a Oauth Key/Secret.
Fortunately you only need to do this once.

Register a new Oauth Consumer on the [Meetup API Oauth Consumer portal](https://secure.meetup.com/meetup_api/oauth_consumers/)

* `Consumer Name` Provide a name for your Oauth Consumer
* `Application Website` Can be pretty much anything, here i used `https://github.com/lazywinadmin/MeetupPS`
* `Redirect URI` Can be pretty much anything, here i used `https://github.com/lazywinadmin/MeetupPS`
* Agree with terms

![image-center](/media/MeetupPS-RegisterOauthConsumer01.png)

Once the Oauth Consumer is created, copy the Key and the Secret. This will be used to authenticate against the API

![image-center](/media/MeetupPS-RegisterOauthConsumer02.png)

<a name="Authenticate"/>

## Connecting to the Meetup.com API

```powershell
# Connect against Meetup.com API
$Key = '<Your Oauth Consumer Key>'
$Secret = '<Your Oauth Consumer Secret>'
Set-MeetupConfiguration -ClientID $Key -Secret $Secret
```

![image-center](/media/MeetupPS-Set-MeetupConfiguration01.png)

Note: This will leverage two private functions of the module:

* `Get-OauthCode`
* `Get-OauthAccessToken`

This will then prompt your to connect to Meetup.

![image-center](/media/MeetupPS-Set-MeetupConfiguration02.png)

<a name="GetGroupInfo"/>

## Get Group information

Retrieve a group information

```powershell
Get-MeetupGroup -Groupname FrenchPSUG
```

![image-center](/media/MeetupPS-Get-MeetupGroup01.png)

<a name="GetEventInfo"/>

## Get Events information

<a name="GetupcomingEventInfo"/>

### Upcoming events

Retrieve upcoming event(s) for a group

```powershell
Get-MeetupEvent -Groupname FrenchPSUG -status upcoming
```

<a name="GetpastEventInfo"/>

### Past events

Retrieve past event(s) for a group

```powershell
Get-MeetupEvent -GroupName FrenchPSUG -status past -page 2
```

![image-center](/media/MeetupPS-Get-MeetupEvent03.png)

```powershell
Get-MeetupEvent -GroupName FrenchPSUG -status past |
Format-List -property Name,local_date,link, yes_rsvp_count
```

![image-center](/media/MeetupPS-Get-MeetupEvent04.png)

<a name="CreateEvent"/>

## Create Event

```powershell
New-MeetupEvent `
    -GroupName FrenchPSUG `
    -Title 'New Event from MeetupPS' `
    -Time '2018/06/01 3:00pm' `
    -Description "PowerShell WorkShop<br><br>In this session we'll talk about ..." `
    -PublishStatus draft
```

![image-center](/media/MeetupPS-New-MeetupEvent01.png)

Here is the event created in Meetup

![image-center](/media/MeetupPS-New-MeetupEvent02.png)

<a name="APIPermissionScopes"/>

## API permission scopes

Note that you need to specify the scope of the actions you want to perform

https://www.meetup.com/meetup_api/auth/#event_management_scope

Here are the current scopes used by the private function `Get-OAuthAccessToken`

```powershell
$Headers = @{
    'X-OAuth-Scopes'          = "basic", "reporting", "event_management"
    'X-Accepted-OAuth-Scopes' = "basic", "reporting", "event_management"
}
```

<a name="Resources"/>

## Resources

* [Meetup API Documentation](https://www.meetup.com/meetup_api/docs/)