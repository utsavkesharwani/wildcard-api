<!---






    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).






-->
<p align="center">
  <a href="/../../#readme">
    <img src="https://github.com/reframejs/wildcard-api/raw/master/docs/images/logo-with-text.svg?sanitize=true" height=106 alt="Wildcard API"/>
  </a>
</p>


&nbsp;

# FAQ

<a href=#how-does-rpc-compare-to-graphqlrest>How does RPC compare to GraphQL/REST?</a>
<br/>
<a href=#which-is-more-powerful-graphql-or-rpc>Which is more powerful, GraphQL or RPC?</a>
<br/>
<a href=#doesnt-rpc-tightly-couple-frontend-with-backend>Doesn't RPC tightly couple frontend with backend?</a>
<br/>
<a href=#should-i-deploy-frontend-and-backend-at-the-same-time>Should I deploy frontend and backend at the same time?</a>
<br/>
<a href=#should-i-develop-frontend-and-backend-hand-in-hand>Should I develop frontend and backend hand-in-hand?</a>
<br/>
<a href=#how-can-i-do-versioning>How can I do versioning?</a>

<br/>

### How does RPC compare to GraphQL/REST?

They have different goals.

With GraphQL/REST you create a *generic API*:
an API that aims to be able to fulfill a maximum number of data requirements;
enabling third parties to build applications on top of your data.
If your goal is to have an ecosystem of third-party applications,
then you need a generic API and you'll have to use something like REST/GraphQL.

With RPC you create a *custom API*:
an API that fulfills the data requirements of your clients and your clients only.
If your goal is to retrieve and mutate data from your frontend,
then Wildcard offers a simple alternative.

We explain this in more depth at
[RPC vs REST/GraphQL](/docs/rpc-vs-rest-graphql.md#rpc-vs-restgraphql).


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>



### Which is more powerful, GraphQL or RPC?

Depends.

From the perspective of a third party,
GraphQL is more powerful.

From the perspective of frontend development,
RPC is more powerful.

With Wildcard,
while developing the frontend,
everything the backend can do is only one JavaScript function away:

~~~js
// Your Node.js server

const endpoints = require('wildcard-api');

endpoints.iHavePower = function() {
  // I can do everything the Node.js server can do
};
~~~
~~~js
// Your browser frontend

const endpoints = require('wildcard-api/client');

// The backend power is one JavaScript function away
endpoints.iHavePower();
~~~

The whole power of the backend is at disposal while developing the frontend.
For example,
any SQL/ORM query can be used to retrieve and mutate data.
That's arguably more powerful than GraphQL.

The distinctive difference is that,
from the perspective of a third party,
the API is set in stone
but,
from the frontend development perspective,
the API can be modified at will.
(Note that RPC assumes that the API can be modified and re-deployed at the whim of the frontend development,
which we talk about at <a href=#should-i-deploy-frontend-and-backend-at-the-same-time>Should I deploy frontend and backend at the same time?</a>)


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>



### Doesn't RPC tightly couple frontend with backend?

Yes it does.
RPC indeed induces a tighter coupling between frontend and backend.
More precisely, RPC increases the need for synchronized frontend-backend deployements.

For example:

~~~js
// This API endpoint is tightly coupled to the frontend:
// it returns exactly (and only) what the landing page needs.
endpoints.getLandingPageData = async function() {
  const user = await getLoggedUser(this.headers);
  if( !user ){
    return {isNotLoggedIn: true};
  }
  const todos = await db.query('SELECT id, text FROM todos WHERE authorId = ${user.id};');
  return {user, todos};
};
~~~

If changes are made to the frontend that require the todo items' creation,
then the SQL query of the `getLandingPageData` API endpoint needs to be changed from `SELECT id, text FROM` to `SELECT id, text, created_at FROM`.
This means that the API needs to be modified and the backend re-deployed.

In general (and regardless whether you use RPC or REST/GraphQL)
we recommand to synchronize your backend and frontend deployment.
Which we talk about in the next querstion
<a href=#should-i-deploy-frontend-and-backend-at-the-same-time>Should I deploy frontend and backend at the same time?</a>.


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>



### Should I deploy frontend and backend at the same time?

Yes, we recommend synchronized deployements, that is to deploy frontend and backend at the same time.

We also recommend to put the frontend and backend code a monorepo.
(A monorepo is a repository that holds the codebase of all different components of a system, instead of having a multitude of repositories each holding the codebase of a single component.
Monorepos are increasingly popular; it makes it easy to perform changes across system components and removes the need to manage dependency between system components.)

A monorepo with synchronized deployment setup
lends itself well in a full-stack JavaScript app. For example:

~~~js
// Our backend's Node.js server
const express = require('express');
const server = express();

// We serve and deploy our frontend over the Node.js server:
server.use(express.static('/path/to/frontend/dist/'));
~~~

This ensures that frontend and backend are always deployed synchronously.
The backends serves only one API version and the served API is always the correct one.


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>



### Should I develop frontend and backend hand-in-hand?

You can, but you don't have to.

Although,
since more and more frontend engineers are full-stack engineers,
it makes sense to hire full-stack engineers and develop frontend and backend hand-in-hand.

You can still have separation of concerns:
- Backend code concerned about the frontend, which includes the API endpoints that run SQL/ORM queries on behalf of the frontend, is developed by frontend developers.
- The rest of the backend, that is agnostic to the frontend, is developed by backend developers.

The strict separation between browser-side code and server-side code makes less and less sense.
Nowadays, most frontend engineers are comfortable and eager to write server-side code.
To a full-stack engineer, RPC is a boon:
it gives him the power to use any SQL/ORM query and any server-side tool he wants.


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>



### How can I do versioning?

As explained in
<a href=#should-i-deploy-frontend-and-backend-at-the-same-time>Should I deploy frontend and backend at the same time?</a>,
we recommend to deploy frontend and backend synchronously.
You then don't need
versioning: the backend always serves a single version (the correct one) of the API.


<br/>

<p align="center">

<sup>
<a href="https://github.com/reframejs/wildcard-api/issues/new">Open a ticket</a> or
<a href="https://discord.gg/kqXf65G">chat with us</a>
if you have questions, feature requests, or if you just want to talk to us.
</sup>

<sup>
We enjoy talking with our users.
</sup>

<br/>

<sup>
<a href="#faq"><b>&#8679;</b> <b>TOP</b> <b>&#8679;</b></a>
</sup>

</p>

<br/>
<br/>




<!---






    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).












    WARNING, READ THIS.
    This is a computed file. Do not edit.
    Instead, edit `/docs/faq.template.md` and run `npm run docs` (or `yarn docs`).






-->