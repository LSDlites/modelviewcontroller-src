# Visibility Allowed Notification

### Motivation

Views that are hidden to the User generally do not need to react to notifications
from the Model: depending on the nature of the View and the optimizations of the
UI toolkit used, this can result in performance degradation from mild to severe. 
Machine cycles can be saved by ignoring the notification altogether, according to
the visibility status. 

One possible approach to this optimization is to unsubscribe the View from the 
Model when hidden, and re-subscribe when shown. This option requires some 
bookkeeping, and is generally less appealing than the alternative:
to simply interrupt further processing of the notification when the View is hidden.
When the View becomes visible again, synchronization with the Model must occur, 
but only if an actual change has taken place. Failure to do so would slow down the
return of the View without reason.

### Design

<p align="center">
    <img src="images/visibility_allowed_notifications/visibility_allowed_notifications.png">
</p>

When a notification is delivered to the View, the View checks for its visibility.
If not visible, it simply sets a `needs_update` flag.

When the View is made visible again, and assuming the View preserves
its visual state even when hidden, the `needs_update` flag is checked. 
If the flag is set, the View resynchronizes against the Model contents,
otherwise, it just presents the old visual appearance.

