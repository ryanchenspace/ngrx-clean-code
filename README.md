# NgRx Clean Code

## Introduction

A list of NgRx concepts, features and a series of guidelines and good practices of how to use NgRx in Agular.

## Repo

Repo with Code: https://github.com/ryanchenspace/ngrx-start

## Core Concepts

### NgRx

NgRx prescribes an architecture for managing the state and side effects in your Angular application. It works by deriving a stream of updates for your applications components called the "action stream".

- You apply a pure function called a "reducer" to the action stream as a means of deriving state in a deterministic way.
- Long runing processes called "effects" use RxJS operators to trigger side effects based on these updates and can optionally yield new changes back to the actions stream.

#### Mental Model

- Select and Dispatch are special versions of Input and Output
- Delegate responsibilities to individual modules of code

``` ts
@Input () books: Book[]   // component way

this.store.select(selectBooks) // NgRx way
```
``` ts
@Output() select: EventEmitter<string>() // component way

this.store.dispatch(selectBookAction(book)) // NgRx way
```

#### Responsibilities

- Containers connect data to components
- Effects tiggers side efffects
- Reducters handle state transitions
- State flows down, changes flow up
- Indirection between state & consumer
- Input & Output => Select & Dispatch
- Adhere to single responsibility principle

### Actions

Actions express unique events that happen throughout your application.

- Unfied interface to describe events
- Just data, no functionality
- Has a minimum type property
- Strongly typed using classed and enums
  
#### Action Type

- Page Action : From  user interaction with the page
- API Action: External interaction through network requests

```ts
export const BooksApiActions = createActionGroup({
    source: 'Books API',
    events: {
        'Books Loaded Success': props<{ books: BookModel[] }>(),
        'Book Created': props<{ book: BookModel }>(),
        'Book Updated ': props<{ book: BookModel }>(),
        'Book Deleted': props<{ bookId: string }>(),
    }
})


export const BooksPageActions = createActionGroup({
    source: 'Books Page',
    events: {
        'Enter': emptyProps(),
        'Edit Book': props<{ bookId: string }>(),
        'Cancle Editing Book': emptyProps(),
        'Create Book': props<{ book: BookRequiredProps }>(),
        'Update Book': props<{ bookId: string, changes: BookRequiredProps }>(),
        'Delete Book': props<{ bookId: string }>(),
    }
})
```
#### Best Pratices

- Unique events get unique actions
- Actions are grouped by their source
- Actions are never reused

#### How to find the events

1. Using  sticky notes, as a group identify all of the evnets in the system
2. Indentify the commands that cause the event to arise
3. Indentiy  the actor in the system that invokes the command
4. Indentify the data models attached to each event
   
### Reducers

- Produce new states
- Receive the last state and next action
- Listen to specific actions
- Use pure, immutable operations
  
### Store

- State contained in a single state tree
- State in the store in immutable
- Slices of state are updated with reducers
  
### Selectors

- Allow use to query our store for data
- Recompute when their inputs change
- Fully leverage memoization for performance
- Selectors are fully composable
  
### Effects

- Processed that run in the backgroud
- Connect your app to the outside world
- Ofen used to talk to services
- Written entirely using RxJS streams
