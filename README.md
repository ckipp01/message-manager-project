# message manager coding project

 Coding project for Chris Kipp

## Build Setup

>Unzip the file and cd into the directory. The best way to view what is going on is to install the dependencies following the instructions below, and then npm run dev. The working project should appear at localhost:8080. You can also do the production build, which will minify everything, but I figured you had more interest in seeing the code itself.

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

### Tech/Design Choices
First, a quick rundown of the main tech I used and why.
* [Vue.js](https://vuejs.org/) - I've worked in vue for the past few months in a cordova app project, and I've really enjoyed it. At first I was going to just tackle this with jQuery and not have to worry about webpack or anything like that, but you said to have fun with it, so vue it is.
* [Element](http://element.eleme.io/#/en-US) - Element is a UI library made for vue. I've had my eye on this for a while and was looking for a little project to play around with it. This seemed to actually work really well as some of the bindings that were included, for example, the selects were prefect. It also saved me a lot of time making sure that I didn't have to do them from scratch.

A few design points.
* I wanted to make a simple UI for the person using this to be able to simply choose the hotel, choose the customer, and then choose the type of message that will be sent out. I chose to interpret your time stamps as when the messages would be sent, so the check in message is based off the startTimestamp and the check out message is based off the endTimestamp.
* Since the UI components were able to store the values of customers, guest, and type of message, it was then pretty easy to make a decision tree that guided the construction of the message the customer would be recieving.

Why Javascript?
* I like UI stuff, so a big part of it was wanting to make sure I could do some of that. Since that was the case and I knew I'd be working in the front end, I just decided to make it fully browser based an do everything in Javascript. It's also what I feel most comfortable with.

## Edge Cases
* One of the main edge cases is actually that you have US/Western as a timezone, which isn't actually a timezone. I cheated a bit and had to add it as a fake timezone to mimic Pacific Time. Ideally this wouldn't happen.
* I have it coded in a way that if one of the fields is not selected, it won't let you generate a message. Hopefully this will deter some of the edge cases with missing fields.

## If I had more time
If I had more time, there would be a few things I'd like to tackle.
* The largest would be testing. The application was pretty small, so I sort of just tried every option to make sure it did what I wanted. In an ideal world, I'd have tests to run for all of this to ensure that edge cases were caught and that everything was behaving the way I wanted it to.
* I have the fake timezone added right now. I'd like to be able to have it where instead, if a timezone appears that isn't valid, it takes the city value and determines the timezone based off that, and then reasigns that value.



