# What's this?
(DRAFT) This is a web app which helps manage and organize summer climbing school.

The web app supports working offline, since the daily school trips are often made in remote areas without an internet connection.

## Who is this made for?
This app is originally made for [AO Željezničar](http://www.aozeljeznicar.hr/) - a climbing club from Zagreb, Croatia

# Database
Data is stored in PouchDB, which enables us to work offline and online.

## Document types
## Doc type: `school-season`
Description: school season / year

Fields:
* `year` = school season year
* `chief-manager` = ID of staff member who acts as chief manager
* `vice-managers` = ID of staff members who act as vide-managers
* `staff`: ID's of staff members who have perticipated in the school season

## Doc type: `area`
Description: a area at which the filed trip is held (i.e. *Paklenica*, *Klek*, *Kalnik*)

Fields:
* `name` = name of the area (i.e. *Paklenica*, *Klek*, *Kalnik*)
* `rescue-info` = contact numbers of the local mountain rescue service
* `logging-info` = contact and other ifno about logging
* `park-info`= (contact) info about the park authority, who operates at the area

## Doc type: `route`
Description: climbing route at an area

Fields:
* `name` = name of the climbing route
* `area` = climbing area ID
* `sector` = name of the climbing sector
* `difficulty` = climbing difficulty (in French or UIAA scale)
* `length` = length of the route in meters
* `info` = additional route info
* `type` = type of the route (*trad* / *sport*)

## Doc type: `exercise`
Description: exercise which participants need to complete

Fields:
* `name` = name of the excercise (i.e. *lead climbing*, *repelling*)
* `notes` = optional notes

## Doc type: `staff`
Description: teaching staff who assists during the school trips

Fields:
* `first-name` = first name of the person
* `last-name` = last name of the person
* `mobile` = mobile phone number
* `email` = e-mail address
* `password` = password used to access the application (if not set the member will not be able to access the app)
* `access-level` = acces level of the user ("assistant",  "trip-manager", "chief-manager", "admin")

## Doc type: `student`
Description: student attending the school season

Fields:
* `first-name` = first name of the person
* `last-name` = last name of the person
* `mobile` = mobile phone number
* `email` = e-mail address
* `exercises` = excercise ID's of excercise completed by the student
* `left-school` = true if the student left/abandoned the school

## Doc type: `school-trip`
Description: a trip made during a school season

Fields:
* `area` = climbing area ID
* `date` = date of the trip
* `trip-managers` = IDs of the staff members who acts as trip managers
* `exercises` = ID's of exercises planned for the trip

## Doc type: `school-trip-staff`
Description: staff attending a school trip

* `trip` = ID of the trip
* `staff` = ID of the staff member
* `present` = which days will the student be present at the trip (*both* / *sat* / *sun*)
* `can-climb` = will/can the staff member climb (*true* / *false*)
* `notes` = optional notes

## Doc type: `school-trip-student`
Description: student attending a school trip

* `trip` = ID of the trip
* `student` = ID of the student
* `present` = which days will the student be present at the trip (*none* / *both* / *sat* / *sun*)
* `exercises` = ID's of exercises done by the student
* `notes` = optional notes about the student

## Doc type: `school-trip-climb`
Description: climb performed during a school trip

Fields:
* `school-trip` = ID of the school trip
* `date` = date of the climb
* `route` = ID of the route
* `students` = ID of the student performing the climb
* `staff` = ID of the staff member supervising the climb
* `student-partner` = (optional) ID of student climbing partner (not set if the staff member is the climbing partner)
* `staff-interview` = a post-climb interview with the staff member
* `student-interview` = a post-climb interview with studnet
* `should-lead` = should the student lead any of the pitches?
* `did-lead` = has the student lead any of the pitches
* `could-lead` = staff member's assesment if the student could leade in the
* `would-lead` = student's assessment if he/she would lead if presented the opportunity
* `manager-notes` = notes made by the manager

# Sitemap
* Season Dashboard
    * Trip Dasboard
        * Staff
            * Lists staff attending the trip
            * New staff can be added from a search box
            * New staff members can be added (if not found)
            * Staff member can be edited
        * Students - list of students attending the trip
            * Each item shows if the student is attending
            * Items can be edited > setting the status (attending/absent)
            * Student info can be accessed (described down below)
        * Exercise
            * Exercises (tab) - list of planned exercise
                * Each item can be expended to show a list of students with the status (done: yes/not)
            * Students (tab) - list of all students at the trip
                * Each item can be expended to show excercise report for the given student (done: yes/no)
        * Climbing
            * Routes (tab) - list of climbing plans pending or in progress
                * Sub-tabs
                    * Planned/inprogress
                        Add button opens a *Climb planner editor*
                    * Done
                * Each item:
                    * contains numeric indicator of climbes pending and ones in progress
                    * can be expanded to show the list of all the attached climbing plans
            * Students (tab)
                * Climbing (sub-tab) - lists only students with climb pending or in progress
                * All (sub-tab) - lists all the students attending the trip
                    * "Assign climb" button displayed next to students not climbing
                    * "Play" button displayed next to students pending to climb
                    * "Done" button displayed next to students with climb in progress
                    * "Interview" button displayed next to students with a finished climb
            * Staff (tab) - list of staff members with a climbing plan pending or in progres
                * Climbing (sub-tab) - lists only staff members with climb pending or in progress
                * All (sub-tab) - lists all the staff members attending the trip
                    * "Assign climb" button displayed next to staffers not climbing
                    * "Play" button displayed next to staffers pending to climb
                    * "Done" button displayed next to staffers with climb in progress
                    * "Interview" button displayed next to staffers with a finished climb
        * *Climb planner editor*
            * Route - combo + add new button
            * Student - combo
            * Staff member - combo + add new button
            * Student partner - combo
            * Should lead - checkbox
        * *Post-climb interview editor*
            * Student comment
            * Staff member comment
            * Did lead
            * Will lead
    * Students
        * New students can be added
        * Each item can be clicked > Student info
            * General (tab) - can be edited
            * Exercises (tab)
                * Total (sub-tab) - list of all the exercise student needs to performe during school
                * History (sub-tab) - list of exercise performed at previous trips, grouped by trip
            * Climbing (tab) 
                * items are groupped by trip
                * each item can be viewed i detail (edit is not posible)
    * Admin
        * New Season
        * Edit season
        * Locations - location list & editor
        * Routes - route list & editor