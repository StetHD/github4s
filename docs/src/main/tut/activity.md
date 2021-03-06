---
layout: docs
title: Activity API
---

# Activity API

Github4s supports the [Activity API](https://developer.github.com/v3/activity/). As a result,
with Github4s, you can interact with:

- [Notifications](#notifications)
  - [Set a thread subscription](#set-a-thread-subscription)

The following examples assume the following imports and token:

```tut:silent
import github4s.Github
import github4s.Github._
import github4s.jvm.Implicits._
import scalaj.http.HttpResponse
// if you're using ScalaJS, replace occurrences of HttpResponse by SimpleHttpResponse
//import github4s.js.Implicits._
//import fr.hmil.roshttp.response.SimpleHttpResponse

val accessToken = sys.env.get("GITHUB4S_ACCESS_TOKEN")
```

They also make use of `cats.Id`, but any type container implementing `MonadError[M, Throwable]` will do.

Support for `cats.Id`, `cats.Eval`, and `Future` (the only supported option for scala-js) are
provided out of the box when importing `github4s.{js,jvm}.Implicits._`.

## Notifications

### Set a Thread Subscription

This lets you subscribe or unsubscribe from a conversation.

Unsubscribing from a conversation mutes all future notifications (until you comment or get @mentioned once more).

You can subscribe or unsubscribe using `setThreadSub`; it takes as arguments:

 - `id`: Thread id from which you subscribe or unsubscribe.
 - `subscribed`: Determines if notifications should be received from this thread.
 - `ignored`: Determines if all notifications should be blocked from this thread.

```scala
val threadSub = Github(accessToken).activities.setThreadSub(5,true,false)
threadSub.exec[cats.Id, HttpResponse[String]]() match {
  case Left(e) => println(s"Something went wrong: ${e.getMessage}")
  case Right(r) => println(r.result)
}
```

The `result` on the right is the created or deleted [Subscription][activity-scala].

See [the API doc](https://developer.github.com/v3/activity/notifications/#set-a-thread-subscription) for full reference.

As you can see, a few features of the activity endpoint are missing.

As a result, if you'd like to see a feature supported, feel free to create an issue and/or a pull request!

[activity-scala]: https://github.com/47deg/github4s/blob/master/github4s/shared/src/main/scala/github4s/free/domain/Activity.scala
