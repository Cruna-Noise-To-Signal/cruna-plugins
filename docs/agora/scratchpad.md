you got what i am after. the interests are just examples of two different spheres (family and work).

what i want to do here is to create a scalable way to allow different people to contribute knowledge to a shared interest and make it so the contribution builds knowledge upon each other. the knowledge is what then drives the action, the decisions, ...

The tools i want to use in order to make information flow are very simple:
- a Shared filesystem
- a Shared Calendar
- an AI client (one for each person involved in the interest) with:
    - one skill or more to interact with the Drive and the Calendar (to allow the user to push and pull information)
    - some routines to push information to the user proactively (todos like: daily review, daily preparation, week in review, week ahead, month in review, month ahead)

The AI client (Claude AI or ChatGpt) is what makes the information organized and usable, the user/s only push information to it (as messages, pdfs, png, jpeg, ...).
Google Drive is the shared filesystem where information is persisted and retrievable when needed,  Google Calendar is the shared calendar the place where appointments, events, placeholders are put.

The Principles: 

* ACCESSIBILE: knowledge must be accessible and readable by both the user and the AI. This means: 
    - everything that is stored must be human readable and reachable (hence .md files are fine)
    - index.md files are what drive the search of the AI through the pile of knowledge enforcing progressive discovery: 
        - the AI first looks into the index.md to get where information are stored
        - then goes and picks only what is needed
* EXTENSIBLE: adding a shared interest as well as booting up the *system* must be as easy as asking the AI client to add an interest and from then onward this can be followed
* MINIMAL and SIMPLE: the design of the folder structure and the file to look into for the user must be simple minimal
* RESILIENT: the system for push information requires some sort of discipline on the user side, the AI must cover for the forgetting of it (mainly using routines)
* BUILD FOR SHARED INTERESTS, WORKS FOR A SINGLE PERSON AS WELL: the filesystem, calendar and AI client (with verbs (aka skills) and habits (routines)) work for multiple user interacting on it (push and pull) but the case of one only person using it is no special, it simply is a group of one

regardless of the kind of interests the shape of the **filesystem** is very simple and recursive:

```markdown
SHARED-DRIVE-FOLDER/
|-- index.md --> indexes the interests folders content with minimal description
|-- status.md --> overall view of the interests (just a glance to each of them, with needs attention)
|-- landing-folder --> general purpose landing folder (see the push section below on Primitive 2)
|-- memories --> AI client memories folder (see section below on Primitive 3)
|-- interest-X/
|---- index.md --> indexes the interest specific instances
|---- status.md --> overall view of the instances (just a glance to each of them, with needs attention)
|---- instance-Y/
|------ status.md --> specific summary from the diary entries and documents of the current situation of the instance (what has been done so far, what is pending, what to do next)
|------ index.md --> index over the documents (especially)
|------ diary/ --> log entry of information pushed by the users
|-------- user-z-yyyymmdd_hhmm.md
|-------- user-w-yyyymmdd_hhmm.md
|------ documents/ --> repository of the documents that are related to the instance
|-------- user-z-yyyymmdd_hhmm_<original-doc-name-doc1>.pdf
|-------- user-w-yyyymmdd_hhmm_<original-image-name-img1>.jpg 
```

the role of the **Calendar** is for the user and the AI to put reminders and appointments into it (it is one way in which the knowledge becomes action).

**Known Limitations**
Attachments in the AI clients chats are not seen as file, so asking the AI client to upload the file to the remote filesystem does not work: the Shared Drive must therefore have a landing folder where the user deposits the files then tells the AI client to move them, or it deposits the file where needed  and then tells the AI client to *index it*.

Here some examples of both from the family and job sphere which give room for interpretation of this framework (there is no right or wrong, however once set up there must be consistent across the filesystem). Both examples present the same primitives of interaction between the user and the system through the AI client.

### Family example

The most common use case for this systems is the two parents to organize and share information, plan ahead and be consistent with whatever deadlines. I use my family in order to make the case explicit.

#### Primitive 1: Booting the system through an interview

At startup of the system there must be the interview phase that makes the system know who is interacting with (the org chart) and mapping the first interests.

##### The Org Chart
The system must know the family it is serving in order to get an understanding of who is who and who is going to interact with it.

**My familiy Org Chart**:
my wife Simona (40 years old, birthday 23 feb 1986)
me Simone (37 years old, birthday 28 march 1989)
elder daughter Nina (4 years old, birthday 23 june 2022)
younger daughter Greta (0 years old, birthday 2026)

with respect to the filesystem presented above this might require a special org.md file describing the organization the system is serving.

##### The First Interests
Then the system must know the first interests it must map in order to organize the filesystem.

**My family First Interests**
health
events to participate to or to organize
ideas for would be nice to visit places / event
house duties (things to pay, to buy, to get rid of)
kid duties (especially for Nina: swimming pool, ski, ...)
Simona and Simone personal calendars and or personal interests

First design question: how to shape the interests? Per person, hence it means:

```markdown
NINA/
|- index.md
|- status.md
|- health/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- sport/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- events/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
GRETA/
|- index.md
|- status.md
|- health/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- sport/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- events/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
SIMONA/
|- index.md
|- status.md
|- health/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- sport/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- events/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
SIMONE/
|- index.md
|- status.md
|- health/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- sport/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- events/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
FAMILY/ -> for shared things like (vacations, ...)
|- index.md
|- status.md
|- health/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- sport/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
|- events/
|-- index.md
|-- status.md
|---- diary/
|---- documents/
```
Or by "interests":

```markdown
HEALTH/
|- NINA/
|- ...
|- GRETA/
|- ...
|- SIMONA/
|- ...
|- SIMONE/
|- ...
SPORT/
|- NINA/
|- ...
|- GRETA/
|- ...
|- SIMONA/
|- ...
|- SIMONE/
|- ...
LEISURE/
|- NINA/
|- ...
|- GRETA/
|- ...
|- SIMONA/
|- ...
|- SIMONE/
|- ...
```
The first one is a bit more boiler plate but i like it more: it is clearer to me.


#### Primitive 2: User pushes into the system and pulls from the system
The user(s) (probably from a shared chatgpt / claude account) interacts with the filesystem through the AI client.

##### Push

For all these cases the AI client must keep the `status.md` (always) and the `index.md` (when needed) updated with respect to the latest pushed information.

**1. Push an information as a diary log**: the user tell something about a concept belonging to an interest.
**2. Push a file**: given the known limitations the user cannot upload the file to the AI client chat interface and expect the client to upload it into the filesystem. Hnece the user will do one of the following:
a. upload the file in the `interest->concept->document` folder and tell the AI client he has done so 
b. upload the file in the `landing` folder at the root of the filesystem and tell the AI client he has done so
c. either a or b without telling the AI client he did it
In cases a and b the AI client updates the files as the user reports he did it, in case c instead the dutie of doing so must be delegated to a routines
**3. Push a reminder or schedule a meeting**: the user asks for a reminder to be set, or to set an appointment, or to create a placeholder for a thing to be done. Also the AI client could infer from a pushed information that a meeting must be scheduled, or some prep time must be set aside. This is why the calendar exists: everything has to be remembered must go through the calendar. in order to make the calendar easy to understand at a glance using color codes or naming convention like `interest-concept-yyyymmdd meeting`
**4. Push artifacts from a pull session**: when the user pulls an information it can engage in a conversation that then asks for pushing something back (sometimes also for a simple update)
**5. Open new interests**: user ask for initiating a new interest, the filesystem scaffolding must be set in place

***Important Principle***: the AI client never assumes implicit knowledge: when it is not clear what the interest, concept, the person pushing the info. IT ASKS the user!

**Status Update Rule**: *less recent logs and docs weight less*. Diary logs and documents are stored with the date in which the entry was made. This is on purpose: at each update of the status on the surface of new information the file content is re-evaluated in order to keep it consistent with the current situation.

##### Pull

Pull is straightforward: the user asks for information about a concept inside the entity. The AI client uses the `index.md` files to drive the discovery of only what is useful and get the actual information from the `status.md`, a `diary-log-entry`, a `document` or a `calendar`. The AI client uses the **Status Update Rule** to return the most relevant information: here relevancy means **on topic** and **up to date**.

#### Primitive 3: the habits of routines. AI client gets proactive

In order to make the system yours it must be proactive in surfacing the information and in knowing the interests and all related stuff. This is achieved with routines (scheduled executions of certain tasks that allow the AI client to behave like a consistent assistant delivering information to the user, and also do some tidy up) and memories (in the form of skills that force the AI client to record important specific traits of the user, novelties and subtelties that arises as the user interacts so that the experience delivered is felt even more personalized).

##### Routines

1. keep the index and status updated: prevents stale filesystem representation. As seen in the case of [push a file](#push) the user might put a file where needed but forget to fire the AI client update, routines are there for this purpose
2. daily/weekly/monthly look back, daily/weekly/monthly look ahead: the whole purpose of the system is to make the information flow across the interested. Hence a refelction on the day/week/month just passed that surfaces what happened, what was DONE, what is till pending helps the users to be on the same page. Same goes forward looking with a day/week/month look ahead where users are prepared for what they are going to encounter. 
 
##### Memories

Memories are a way for the AI client to get more familiar with the users, in order to grasp novelties that might be related to the user it self or to an inerest or concept while talking about somethnig else. These is like an always on skill (also reminded about in the other skills) that pushes the AI client to assess what the user says in the light of what it knows about the user. If the AI client can access the conversations, this can also run as a routine each day in which the AI client reflects on the conversation it had with the users and stores relevant information about it.

---
I do not go into the details of all primitives examples for the family case (i think they are obvious), further i do not expand an example about the work sphere (what i would have done was targeting the usual family run artisanal business with 2-5 employees where the shared interests are the clients and the works they are doing for them, and make sure that prospects become clients).

That being said i think this mental framework applies nicely to any kind interest and concept, and to any small size of users involved (from 1 to 3/4/5).

**OUT OF SCOPE: but future extensions**
I already see extensions of it from a tooling perspective:
- if dealing with code and engineers the shared drive can be paired with a git repositories, interests can become repositories upon which engineers and developers code and the AI client keeps status aligned and memories aligned reflecting on the commits, the pr, the open issues
- each person can setup this filesystem (even multiple of this) on her laptop (when you are not sharing info with anyone else the local filesystem does just fine, if you can access it from everywhere)

What i want to do is to lock the *data modeling*, define and create the skills and routines (for both ChatGpt and Claude AI) that support this general vision of what a shared interest requires to function. Make sure you grill me if needed on this.