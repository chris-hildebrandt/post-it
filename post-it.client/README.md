CodeWorks Vue Starter
=====================
This template is designed to help get students started building vue applications

## Legal Overview

The content under the CodeWorks®, LLC Organization and all of the individual repos are solely intended for use by CodeWorks Instruction to deliver Educational content to CodeWorks Students.

---

## Copyright

© CodeWorks® LLC, 2021. Unauthorized use and/or duplication of this material without express and written permission from CodeWorks, LLC is strictly prohibited.


<img src="https://bcw.blob.core.windows.net/public/img/7815839041305055" width="125">

lecture notes:

don't npm i into the top level, you must do it at the client and server levels each individually

open project in workspace mode, then update the env.js both in the client and the server.

test your debug server and make sure that front end and backend are communicating. logging in will check this.

do an init commit it may ask which folder you want to push to github, choose folder and backup a level to select the entire project instead of just the client or the server (this may be an unnecessary step now)

review uml diagram and figma

start on the server don't swap back and forth between the client and the server because it will get confusing. the client folder inside the server is the localhost:3000 that allows you to log in and get tokens. if you are not going to use it to get tokens (you can get them from the 8080) then you can delete the server client folder. the bearer tokens are for postman to quickly and easily create data for testing

we start in the server models, import schema from mongoose and export const x = new schema, follow the value model for the syntax, follow the uml diagram for the props your model needs. remember to do the special import for creatorId, and since it is referencing another schema you need to have ref: 'Account' in the {}

create a virtual if you need one, virtuals are for tying relationships together for the name 'creator' we want to tie the localfield: 'creatorId' to the foreignfield: '_id' from that user. this foreign field references 'Account' so put in a ref: 'Account' justOne: true this makes just one array to store these virtuals in. the virtual is information about the schema and the other relation that is stored on the other object so that we don't have to store a copy of the full object in both places. 

now make the controller on the server. extend the BaseController this can be exported from utils, again refer to the valuesController make a constructor and place the super inside of it. the super is the constructor of the extended thing(basecontroller in this case), the super gets the ('api/whatever') which registers the door. then this.router is used to register the different functions/requests remember crud! 

.post goes in this.router,

then write the actual function, is this function accessible by everyone? should it be? place .use(auth...) to prevent access to some features, and it also supplies the user account info to the request. userInfo.id is obtained from auth0 and can be used in the function to add it to the req.data.creatorId

 go to postman to create some data and test our api postman tests will be a new tool we can use during lab and checkpoints

DO NOT CHANGE THINGS IN THE TEST!! the only thing you change in the test is providing an auth bearer token. don't worry about unresolved variables, they are created during the test.

tokens are obtained after login from the network tab in dev tools, put it in the current value in the variables tab, make sure to save.

start server prior to test, run the test many times, test after each function! 

now go to the service and finish the function started in the controller. follow each function through to the end.

zoom: 




day 2:

working on the client side, make sure all the server files are closed to avoid confusion

make an album form component, vt, make a form div do the @submit.prevent="handleSubmit" used a snippet to get a basic form, place the component div in this case <AlbumForm/> in the homepage to look at it
do a little styling to make it useable 

variables in scss lets us change the colors of bootstrap built int colors. remember lighten, darken, and opacity bootstrap

we make sure the form is saveable by making an editable which looks like const editable = ref({})
make sure to import ref! return the editable so it can be used. then v-model your inputs. the server takes care of the ids, and archived will be defaulted to false, and we don't want the user to have the option to start an album out as archived.

the create album method is made in the return, and is async. use the new vue dev tool to look at the form and other elements which update in real time! 

send the method to the service since we are not overwriting the data in our appstate, we want to push the new album into the array, post it to the server, then push or unshift it to the appstate, unshift will put it at the top of the page. this doesn't fix the way the server stores the info though, so we have to go after the .find()you can do a .sort(createdat: -1).populate...
will list the items newest to oldest. -1 is shorthand in mongoose for smaller to larger numbers. 1 and -1 and a-b are used for these sorting methods.

now we put the form in a modal you don't have to move the modal target outside of the component, but you do need the <AblumForm/> div on what ever page the button is at. if you have the modal button in the navbar you need to have the tag accesible at that level or the button won't work on all pages. 

start the filter bar which is placed on the homepage, get buttons labled for each filter term. you need to have the filter in the same location as the v-for. we don't generally want to modify what is in the appstate because if you remove objects and then filter again you will have diminishing returns

filter syntax looks like @click="filterTerm = 'whatever'" then we want to filter the computed, not the appstate. return filterTerm so it is accessible, then simply place a .filter inside the computed .filter(a=>filterTerm.value ? a.category == filterTerm.value: true) this ternary allows an empty filter to return true for all categories don't forget to make a ref for the filterTerm, which in this case is an empty string. you can filter out a category by using the != instead of the ==

make a new page called the album details page, a new page needs to be registered in the router determine the level of security.

then back on the startpage you set up either a router push from the js or a router-link in the html. the routerlink syntax <router-link to: "{ name: 'whateverpage', params: {whateverid: whatever.id}}"

takeaway from today's lecture: user router.push instead of router-link

using router push you need to import router, and const router = useRouter() if you want to be moved you use the router. the router push method will not have access to the id though so you must push props into the setup!

now that we are at the album details page we need make an onmounted and a getAlbumById, which we have to pass an Id, the Id is in the url when we navigate to the page so that is where we need to get it from. this is good because it doesn't get cleared out on a reload like the appstate does. this is where we will do the useRoute() function! in the setup useRoute so that we can put the route.params.id in our getAlbumById function. on the service side do a get to the api, + id. now in the appstate make a variable to save the active album and then sae the res do a computed for the active album
pages need to be self sustaining. the pages act like controllers for their own responsibilities.

now you want to get pictures by album id this is automatic so it goes in the setup, and probably onmounted as well. this means we will need to make a picturesservice. pass the route.params.albumid do a computed for the pictures so you can draw them using a v-for on a pictureCard component. vfor in the parent div with the PictureCard div as the child you can actually put the v-for in the PictureCard dive if that's the only thing you are passing the advantage of using the parent is you can start every card off in the row.. pass the props to the picturecard, and since there isn't a model for "picture" the type of prop is picture in the card use an img tag with a bound ":" src="picture.imgUrl"

the key in the v-for is just something unique that labels the object for vue so that if it is edited or deleted vue can update only that item. you can technically just use the index and you can even v-for without a key at all but this isn't a good practice.

when we create a new picture, we need to attach the album id to the payload this has to happen in the controller so before send it off to the servie we need to editable.value.albumId = route.params.albumId 

many to many: making collaborators on the server side, make a schema the album will have an Id and we will call the creatorId the AccountId because in the context of a many to many we want to make sure that the label is tied to the model they are pointing to.
the properties are just the albumId and the AccountId, but then we need to make virtuals. we make one for album Id local field is the album id foreign is _id and the ref is Album, the account virtual is labeled 'profile' because it is looking at other peoples accounts, not just the users, the virtual is called a profile, but the local field is still accountId and the ref is Account

controller for collaborators is next. we need a service as well, we need to register the model in the dbcontext, follow the test order in the postman tests for creating the methods.we need a post method so we start building the controller by setting up our BaseController extension and put in a ctor with a super which gives the route of /collaborators and then we put our methods under this.router. place a get collaborators in the albumcontroller then get those collaborators by a single album id in the collabcontroller the find in this function does not use an array method or a callback(c=>c.id==id) instead we use an object find method which looks like this find.({albumId: albumId}).populate('profiles') we don't need to populate the album because we are searching by one album so we don't need the id again.

make the method for finding collaborator you collab with by account id, this function is only usable by a logged in user so the account can only be one thing. 

to remove a collaborator we delete the many to many object by the many to many id. generic id param makes sense if it is in the service of the thing with an id the more basic delete will allow one user to remove only themselves from a collab.