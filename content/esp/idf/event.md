---
title: "Event"
date: 2023-05-22T11:13:38+04:00
draft: true
tags:
  - "esp"
  - "idf"
---
## The event loop library

1. Allows you to declare events and their handlers to be called when the event occurs
2. Allows loosely coupled components
3. Simplifies event processing by serialising and deferring code execution to another context

---

Also should say that event used by high level libraries like a WiFi library.

## Events and event loops

To use events you need to

1. Define a function with the same signature as `esp_event_handler_t`
2. Create an event loop with `esp_event_loop_create()`, which returns an `esp_event_loop_handle_t` ( You can also use `esp_event_loop_create_default()` )
3. Register event handler to the loop using `esp_event_handler_register_with()`
4. Event sources post an event to the loop using `esp_event_post_to()`
5. Ureginstering event handler from the loop using `esp_event_handler_unregister_with()`
6. Delete unneeded event loop via `esp_event_loop_delete()`
