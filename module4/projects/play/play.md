---
layout: page
title: Play Play - Full-Stack
subheading: A song playlist creator project for Module 4 BE
---

## Learning Goals

* Create an Express API given specified endpoints and response formats.
* Create a front-end to consume the Express API built for Play.

### Requirements Overview

You will be creating a full-stack application, built in two parts. The first will be the backend portion. Once you have completed your API, you can begin to build out the frontend. You will be creating an application that users will utilize to track songs and playlists.

You will use and consume the [Musixmatch API](https://developer.musixmatch.com/) to create your own API.

* Your backend should be deployed with Heroku.
* Your frontend should be deployed with [GitHub Pages](https://pages.github.com/) or [Surge](https://surge.sh/).

### Front-end user workflow and overview

1. User clicks on a search bar input and enters text of a particular musical artist (i.e Queen). The user then clicks the search button or can hit enter on the keyboard to start the search.
2. When executing the search, a list of songs are populated by the API and is presented in the browser asynchronously.
3. Within the song list, a user can click and star songs they want to favorite.
4. When a song is favorited, the song information is stored in a database.
5. A user can see all of their saved (favorited) songs on the index page of the application.
6. A user can click the "Add to playlist" button once songs have been added as a favorite. This button is hidden until songs have been favorited.
7. Clicking add to playlist will add favorited songs into the newly created playlist
8. Clicking the title of a playlist will take the user to the list of songs in the playlist

It is required that each group complete one (or both!) of the following extensions:

#### Extension Option 1:
1. User can click into each song in a playlist and find lyrics, images, and additional information on the particular song.

#### Extension Option 2:
1. Rank your saved playlists by using the song ranking value that comes from the API.

Main page will show the following:

1. List of favorited songs
2. List of playlists (ranked or not)
3. Results of an artist search. This section should be hidden when no search has been conducted. This section should populate once a search has been made and disappears upon each page refresh.   

#### **Manage Favorites**

- CRUD favorites with basic information, such as name, artist name, genre, and song rating.

#### **Playlists**

At a high level, developers will be able to:

- CRUD playlists
- Add a favorite to a playlist
- See data persist across refreshes
- Manipulate data asynchronously

**A couple of things to note:**
* User should NEVER have to 'refresh' the page to get updated data, and user should NEVER have to type something into the URL bar once they are on your page.

### Back End Features

For your Express back end:

- You'll start the repository from scratch.
- Create an agile board and write stories for each of your endpoints.
- In your agile cards, include what the data should look like in the request and response bodies.
- Send your assigned PM a link to your agile board and your repository.

Take you time to plan out your endpoints and any additional endpoints that may be required for this project.

#### Favorite Endpoints:

**GET /api/v1/favorites**

Returns all favorited songs currently in the database.

The index of favorites will be returned in the following format:

```js
[
  {
    "id": 1,
    "name": "We Will Rock You",
    "artist_name": "Queen"
    "genre": "Rock",
    "rating": 88
  },
  {
    "id": 2,
    "name": "Careless Whisper",
    "artist_name": "George Michael"
    "genre": "Pop",
    "rating": 93
  },
]
```

**GET /api/v1/favorites/:id**

Returns the favorite object with the specific `:id` you've passed in. A 404 is returned if the favorite is not found.

```js
[
  {
    "id": 1,
    "name": "We Will Rock You",
    "artist_name": "Queen"
    "genre": "Rock",
    "rating": 88
  }
]
```

**POST /api/v1/favorites**

This end point creates a new favorite for your database. Please note that the rating system should only allow for a number between 1-100.   

To create a new favorite, use the following parameters:

```js
{
  "favorites": {
    "id": 1,
    "name": "We Will Rock You",
    "artist_name": "Queen"
    "genre": "Rock",
    "rating": 88
  }
}
```

If a favorited song is successfully created, the item will be returned. If the favorite is not successfully created, a 400 status code will be returned. All fields are required.

**PUT /api/v1/favorites/:id**

Allows one to update an existing favorited song with the following parameters:

```js
{
  "favorites": {
    "id": 1,
    "name": "We Are the Champions",
    "artist_name": "Queen"
    "genre": "Rock",
    "rating": 77
  }
}
```

If a favorite is successfully updated (All fields are required), the favorite item will be returned. If the favorite is not successfully updated, a 400 status code will be returned.

**DELETE /api/v1/favorites/:id**

Will delete the favorite with the id passed in and return a 204 status code. If the favorite can't be found, a 404 will be returned.

#### Playlist Endpoints:

**GET /api/v1/playlists**

Returns all the playlists in the database along with their associated favorited songs

If successful, this request will return a response in the following format:

```js
[
    {
        "id": 1,
        "playlist_name": "Favorite songs of all time",
        "favorites": [
          {
            "id": 1,
            "name": "We Will Rock You",
            "artist_name": "Queen"
            "genre": "Rock",
            "rating": 88
          },
          {
            "id": 2,
            "name": "Careless Whisper",
            "artist_name": "George Michael"
            "genre": "Pop",
            "rating": 93
          }
        ]
    },
    {
        "id": 2,
        "name": "Other amazing songs",
        "favorites": [
          {
            "id": 1,
            "name": "We Will Rock You",
            "artist_name": "Queen"
            "genre": "Rock",
            "rating": 88
          },
          {
            "id": 2,
            "name": "Careless Whisper",
            "artist_name": "George Michael"
            "genre": "Pop",
            "rating": 93
          },
        ]
    },
]
```

**GET /api/v1/playlists/:playlist_id/favorites**

Returns all the favorites associated with the playlist with an id specified by :playlist_id or a 404 if the playlist is not found

If successful, this request will return a response in the following format:

```js
{
    "id": 1,
    "playlist_name": "Favorite songs of all time",
    "favorites": [
      {
        "id": 1,
        "name": "We Will Rock You",
        "artist_name": "Queen"
        "genre": "Rock",
        "rating": 88
      },
      {
        "id": 2,
        "name": "Careless Whisper",
        "artist_name": "George Michael"
        "genre": "Pop",
        "rating": 93
      }
    ]
}
```

**POST /api/v1/playlists/:playlist_id/favorites/:id**

Adds the favorite with :id to the playlist with :playlist_id

This creates a new record in the Playlist Favorites table to establish the relationship between this song favorite and playlist. If the playlist/favorite cannot be found, a 404 will be returned.

If successful, this request will return a status code of 201 with the following body:

```js
{
    "message": "Successfully added SONG_NAME to PLAYLIST_NAME"
}
```

**DELETE /api/v1/playlists/:playlist_id/favorites/:id**

Removes the favorite with :id from the playlist with :playlist_id

This deletes the existing record in the Playlist Favorites table that creates the relationship between this song favorite and playlist. If the playlist/favorite cannot be found, a 404 will be returned.

If successful, this request will return:

```js
{
    "message": "Successfully removed SONG_NAME from PLAYLIST_NAME"
}
```


<!-- ### Expectations -->

- Tag instructors in pull requests on Github wherever you'd like feedback.
- Reach out for extra support if you feel like your team is falling behind.
- If there's any question about functionality, ASK.
- To submit, tag your instructor in a PR using [this template](https://gist.github.com/ameseee/c4f0b2e1bb3f41661a7de8574ba3992c).

## Rubric

You will be graded by an instructor on the criteria in this [rubric](./play_rubric).
