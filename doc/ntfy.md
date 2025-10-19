ZNC Push via ntfy
=================

This section contains the specific steps to configure for [ntfy][] after you install the
module by following the above steps.

## What is ntfy?

[ntfy][] (notify) is a simple HTTP-based pub-sub notification service. You can send notifications
to any topic, and subscribe to them via web, phone apps, or any HTTP client. No authentication is
required for public topics (the default), making it very easy to use.

## Basic Setup

1. Choose a topic name for your notifications (e.g., `my-irc-alerts`). Topics are public by default.
   * You can make a topic private using access control, but the default is public and requires no setup.
   
2. Set the service to 'ntfy':

        /msg *push set service ntfy

3. Set the ntfy host (e.g., `ntfy.sh` for the public instance or your self-hosted instance):

        /msg *push set ntfy_host ntfy.sh

4. Set the target (topic name):

        /msg *push set target my-irc-alerts

5. (Optional) Set authentication token if your ntfy instance requires it:

        /msg *push set secret your-ntfy-bearer-token

6. (Optional) Customize the title and tags:

        /msg *push set message_title "IRC: {context}"
        /msg *push set ntfy_tags "irc,highlight"

7. (Optional) Set the priority level using the standard message_priority:

        /msg *push set message_priority high

## Configuration Options

### Required

* `ntfy_host` - The ntfy server host (e.g., `ntfy.sh`, or your self-hosted instance)
* `target` - The ntfy topic to send notifications to (e.g., `my-irc-alerts`)

### Optional

* `secret` - Bearer token for authentication if your ntfy instance requires it
* `message_title` - The notification title (default: `{title}`, supports keyword expansion)
* `message_priority` - Priority level: `min`, `low`, `default`, `high`, `max`
* `ntfy_tags` - Comma-separated tags for the notification (optional, no default)

## Examples

### Basic setup with public topic
```
/msg *push set service ntfy
/msg *push set ntfy_host ntfy.sh
/msg *push set target irc-notifications
```

### With custom priority and tags
```
/msg *push set service ntfy
/msg *push set ntfy_host ntfy.sh
/msg *push set target irc-alerts
/msg *push set message_title "IRC Alert: {context}"
/msg *push set message_priority high
/msg *push set ntfy_tags "irc,urgent,{nick}"
```

### With authentication token
```
/msg *push set service ntfy
/msg *push set ntfy_host ntfy.example.com
/msg *push set target my-alerts
/msg *push set secret tk_AgQdq7mVBoFD37zQVN29RhuMzNIz2
```

### Self-hosted instance with auth
```
/msg *push set service ntfy
/msg *push set ntfy_host internal.example.com
/msg *push set target my-channel-alerts
/msg *push set secret your-bearer-token
/msg *push set message_priority high
/msg *push set ntfy_tags "irc,internal"
```

## Testing

Send a test notification:

    /msg *push send Test notification from ZNC

## Viewing Notifications

* **Web**: Visit `https://ntfy.sh/your-topic-name` in your browser
* **Desktop**: Use one of the [available ntfy clients](https://docs.ntfy.sh/subscribe/cli/)
* **Mobile**: Use one of the available apps (Android: ntfy app, iOS: available in app stores)
* **CLI**: `curl https://ntfy.sh/your-topic-name`

## Security Considerations

By default, ntfy topics are public and anyone knowing the topic name can send/receive notifications.
For private topics on the public ntfy.sh instance, see [ntfy access control documentation](https://docs.ntfy.sh/config/#access-control).

[ntfy]: https://ntfy.sh/
