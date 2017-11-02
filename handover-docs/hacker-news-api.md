# Hacker News API

This is the Hacker News api containing detailed description on how to inertact with the api.
</br>
</br>  

## Items

Stories and polls. They're identified by their ids, which are unique integers. All items have some of the following properties:

| Field | Description | Type |
| --- | --- | --- |
| id | The item's unique id. | String |
| parent | The comment's parent id. The parent could be  another comment or a story | String |
| username | The username of the item's author | String |
| type | The type of item. i.e. "story, comment, poll" | String |
| title | The title of the story, poll or job | String |
| text | The comment, story or poll | String |
| url | The URL of the story | String |
| karma |
| upvotedBy |
| downvotedBy |
| created_at |
</br>  

An example of a story:

```javascript

{
    "id": "59f87c655b1dc209f10c0048",
    "username": "Frank",
    "post_type": "story",
    "post_title": "Student Guide 102",
    "post_text": "Good  stuff",
    "post_url": "http://www.google.com",
    "post_parent": ""
}

```

An example of a comment:

```javascript

{
    "id": "57f87c655b1dc209f10c6527",
    "username": "BunnyTheRabbit",
    "post_type": "comment",
    "post_title": "The Worlds Greatest Title",
    "post_text": "Awesome commnt text",
    "post_url": "http://www.yahoo.com",
    "post_parent": "59f87c655b1dc209f10c0048"
}

```  

Obs: Regarding other required post fields.

In the assignment we are specifically told that our backend is supposed to handle fields like `hanesst_id`, `post_parent`, `username` and `password`. Here is an exmaple provided by the assigment:

```sh

{
	"post_title": "Y Combinator", 
 	"post_text": "", 
 	"hanesst_id": 1, 
 	"post_type": "story", 
 	"post_parent": -1, 
 	"username": "pg", 
 	"pwd_hash": "Y89KIJ3frM", 
	 "post_url": "http://ycombinator.com"
}

```

Our API is able to digest this kind of request, and the fields will be stored in the Post object. But we do not really use `hanesst_id`, `post_parent`, `username` and `password` to anything other than making sure that our api meets the assignment requirements.

Th Post object creates it's own unique id called `id` (not the hanesst id) and another field called `parent` (not post_parent). These values are of type String. So whenever our frontend makes request to the backend we will never use `hanesst_id`, `post_parent`. We will use our own fields/attributes.   

## User endpoints

Authentication

| Nr. | Method | Endpoint | Description |
| --- | --- | --- | --- |
| 1 | POST | /login | Login a user |
| 2 | POST | /user/signup | Sign up a new user |
| 3 | GET | /user | Get user by username |
</br>

Example 1 - Signup:

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "username" : "username",
    "password" : "password"
}'  http://{ip_address}:{port_number}/signup
```

Example 2 - Login

```sh
curl -H "Content-Type: application/json" -X POST -d '{
    "username" : "username",
    "password" : "password"
}'  http://{ip_address}:{port_number}/login
```

Example 3 - Get user by username:

```sh
curl -X GET http://<ip-address>:<post>/user?username=<username>
```

When you log in successfully you will receive a Token in the response header looking like this `Authorization: Bearer  xxx.yyy.zzz`

Important `/signup` status codes

| Code | Description |
| --- | --- |
| 200 | Success |
| 400 | Bad crednetials. User entered incorrect input |
| 500 | Server error |

Important `/login` status codes

| Code | Description |
| --- | --- |
| 200 | Success |
| 401 | Bad credentials. Incorrect username or password |
| 500 | Server error |

## Post endpoints

| Nr. | Method | Endpoint | Description | Return body | Requires authentication |
| --- | --- | --- | --- | --- | --- |
| 1 | GET | api/post?page=<page_number> | Get all posts. | List og post objects. If no results you receive an empty list. | No |
| 2 | GET | api/post?id=<post_id> | Get single post by id and all child posts | Single post object and a list of comments if any | No |
| 3 | POST | api/post | Create new post | void | Yes |
| 4 | POST | api/post/vote | Upvote or downvote a post | void | Yes |
| 5 | PUT | api/post?id=<post_id> | Update post by id | void | Yes |
| 6 | DELETE | api/post/id=<post_id> | Deletes post by id | void | Yes |
</br>

Example 1 - Get all posts:

Obs: You have to specify the page number starting from 1. Default page size is 20. Every page request will return 20 or less posts. This way you can optimze perfomance of your website.

```sh
curl -X GET http://<ip-address>:<post>/api/post?page=1
```

Example 2 - Get single post by id and all child posts:

```sh
curl -X GET http://<ip-address>:<post>/api/post?id=dqwdh12e8ewdwjshjk
```

Example 3 - Create new post:

Obs: Remeber to switch the token to a valid token. The one provided will not work

```sh
curl -H "Content-Type: application/json" -H @{'Authorization'='Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTUxMDM0MjU0NiwidXNlcm5hbWUiOiJ1c2VybmFtZSJ9.wpBvdhT8-wsp4GfuTIrROCHjK6Vp1ySXJZpNFLT9xcvSQcAPDNLHXkHdc-RPaZC7fwPOlvdFkgrRz1DbEa03sj'} -X POST -d '{
	"post_title": "My crazy awesome title here", 
	"post_text": "", 
	"post_type": "story", 
	"post_parent": -1,
	"post_url": "http://www.google.com"
}'  http://<ip-address>:<post>/post
```

Example 4 - Upvote or downvote a post:

Obs: the `mode` parameter in the body can either be `up` or `down`.

```sh
curl -H "Content-Type: application/json" -H @{'Authorization'='Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VybmFtZSIsImV4cCI6MTUxMDM0MjU0NiwidXNlcm5hbWUiOiJ1c2VybmFtZSJ9.wpBvdhT8-wsp4GfuTIrROCHjK6Vp1ySXJZpNFLT9xcvSQcAPDNLHXkHdc-RPaZC7fwPOlvdFkgrRz1DbEa03sj'} -X POST -d '{
	"post_id" : "post_id_here",
	"username" : "current_authenticated_users_username",
	"mode" : "up"
}'  http://<ip-address>:<post>/api/post/vote
```

Example 5 - Update post by id:

```sh
curl -H "Content-Type: application/json" -X PUT -d '{
    "post_title": "New awesome title"
}'  http://<ip-address>:<post>/post?id=q38w4fyiesldhflskedfhk
```

Example 6 - Delete post by id:

```sh
curl -X DELETE http://<ip-address>:<post>/post?id=982347r87whf47xcf
```

## Latest Digested Post endpoint

You can retreive the latest digested post by sending a request to the following ednpoint:

```sh
curl -X GET http://<ip-address>:<post>/latest
```

## Providing Status Information endpoint

You can check the current status of the api by sedning a request to the following endpoint:

```sh
curl -X GET http://<ip-address>:<post>/status
```

The response can either be `Alive`, `Update` or `Down`.

- `Alive` the api is up-and-running
- `Update` the api is been updated
- `Down` the api is down for some unknown reason
