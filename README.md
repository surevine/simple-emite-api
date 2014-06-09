# simple-emite-api

Provides a simple plain-javascript interface for emite to allow implementors to easily create rich chat applications without all this tedious mucking about with GWT


# API Documentation


This API provides a single root level object named ```__chatApplication``` of type "Chat Application", sometimes referred to as the "root object" or "root scope".  This object then contains factories, functions, scopes and collections which can be used by the caller to create a rich chat application in whichever Javascript framework they prefer.

This API is documented on a type-by-type basis below.  See "Examples" for usage information.

**Note on functions / callbacks:**  When documenting function prototypes where a parameter of a specific type is required, we have adopted the convention of providing the type of the parameter in the prototype.  Although not correct Javascript, this makes the documentation easier to understand.

## Chat Application

**Scope:**  ```__chatApplication```

### Properties

| Property |  Type | Description |
| -------- | ------------- | ----------- |
| **User** | Scope of the [User](#User) type | Provides methods for retrieving and manipulating information on chat users |
|

## User

Objects of this type describe an individual user of the system, regardless of whether that user is a [Contact](#Contact) of the current user.  Within the scope of this type, functions properties are provided to retrieve and manipulate users, and to process changes in their [Status](#Status)

**Scope:**  ```__chatApplication.User```

### Properties

| Property | Type | Description |
| -------- | ------------- | ----------- |
| **name** | **Text** | Human-readable name of this user.  ontrast **nickName** in [Contact](#Contact) |
| **JID** | **Text** | The JID of this user, as defined by RFC6122 |
| **organisation** | **Text** (Optional) | If present, names the organisation a user is from.  This is to allow different organisations collaborating within the same user space to namespace their users independantly|
| **status** | [Status](#Status) | The last-observed Status of this user to this client.  Note that other clients may observe a different status, and that this status may not be entirely up-to-date, although it will be updated automatically when this client recieves a Presence Change notification from the configured XMPP server |
| **isCurrentUser** | **Boolean** | If true, then this user object represents the current user |
| **currentUser** | [User](#User) (Global) | The currently logged-in user |
| **settings** | [Settings](#Settings) | This users' Settings and preferences.  Changes to this object will not be transmitted to the server, and take permenant effect (where applicable), until the saveUserSettings function has been invoked.  If the user does not represent the current user, this will be set to **null**  |

### Functions

| Name | Prototype | Return Value | Description |
| -------- | ------------- | ----------- | ----------- |
| **getUser** | ```function __chatApplication.User.getUser(**Text** jid)``` (Global) | [User](#User) | Get a user object for the supplied JID, or **null** if that user is not on the current user's roster, or does not exist.  See [Search](#Search) for details on finding users not on the roster |
| **saveSettings** | ```function __chatApplication.User.saveSettings``` | Persists the given user's [Settings](#Settings), including any additional properties defined.  If the current user does not have permissions to set these settings, then invoking this function does nothing |


### Callbacks

| Name | Prototype | Return Value | Description |
| -------- | ------------- | ----------- | ----------- |
| onPresenceChange | ```function __chatApplication.User.onPresenceChange([User](#User) user, [Status](#Status) status)``` | void | Implement this function to perform processing when a user's Status changes.|

## Settings

This object defines a users' settings and preferences.  In addition to the fields below, implementors may define additional properties of objects of this type, which **will** be stored via calls to ```saveSettings``` on the [User](#User) object.

**Scope:**  ```__chatApplication.User.Settings```

### Properties

| Property | Type | Description |
| -------- | ------------- | ----------- |
| user | [User](#User) | The user these settings apply to (usually the current user) |

### Functions
| Name | Prototype | Return Value | Description |
| -------- | ------------- | ----------- | ----------- |
| save | ```function __chatApplication.User.Settings.save()``` | void | Equivilant to ```this.user.saveSettings()``` |

## Status

Objects in this scope describe the status of a particular user at a particular time.  Note that the properties of a [Status](#Status) object instance do not update as the relevant [User](#User)'s status changes - instead, the value of the [User](#User)'s ```status``` property will change to refer to a different instance of this type.





