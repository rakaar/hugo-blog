---
date: "2020-06-21T00:00:00Z"
subtitle: Understanding difference between web and application server
tags:
- server
title: What is difference between nginx and react server
---

## Background
Once [Pankaj Kumar](https://github.com/shankusu993) expressed, *"What exactly does nginx do?Is it even necessary? How is it different from react server that starts when you run `npm run start` in the terminal"*.
Though I have used both of them in the past, I couldn't exactly answer his question. It is at these moments that you realize that your knowledge is fragile and you don't have a proper understanding of things.
I tried some googling and went through few pages, [this](https://stackoverflow.com/questions/936197/what-is-the-difference-between-application-server-and-web-server) was really helpful. The following is an answer, which I tried to put in my own words to improve my understanding
**If any mistakes/improvements needed, do reply**

## Answer
```
both of them do the same job right - server HTML content when a HTTP request is put

except that there is a difference between how each of them do
if u see what Nginx does
it uses the contents of build directory, and all of them are static files
it means when u put a HTTP request to the server, Nginx serves the files which are already stored


on the other hand, what our react server(even consider flask, django server)
they serve the same content on the fly.
what on the fly means is - whenever u make changes to the application code, they make the changes in the files and serve them.
this is important while application development because we keep changing things often. So better make a file and serve it whenever a change is made
rather than making a static version of your whole application code and serving it .What is wrong in this is - generating static content is time taking , u might have seen how long npm run build takes. though the process of serving files is fast the process of generating files is hell slow

hence you might have figured out why we do npm run start  while developing and use npm run build while production

there is a nice name given in the industry , one is called web server and the other is called application server !
```

## NOTE
Nginx is not only a web server, it does lot of other things too. I mentioned only the web server functionality, as it was the main thing which we were discussing.
