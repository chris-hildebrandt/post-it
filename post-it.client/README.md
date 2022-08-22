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

now make the controller on the server. extend the BaseController this can be exported from utils, again refer to the valuesController make a constructor and place the super inside of it. the super is the constructor of the extended thing(basecontroller in this case), the super gets the ('api/whatever') which registers the door. then this.router is used to   go to postman to create some data and test our api 