# somtodayjs

This module can be used to connect to the somtoday api.
Currently all methods work exept the grades method. (I can't test this because my school has some issues with the grade functionality of somtoday)

## Quick start

install the module by typing

`npm install @egidiusmengelberg/somtodayjs`

After installing you can import the module and use it

```javascript

const Somtoday = require('@egidiusmengelberg/somtodayjs')

let som = new Somtoday()


// get a list of all the available organisations
// you can use these to find out your schools uuid
Somtoday.getOrganisations()
  .then(organisations => console.log(organisations))

// get the acces_token and refresh_token
som.authenticate(uuid, username, password)
  .then(response => {
    // set the necessary propterties
    som.access_token = response.access_token
    som.refresh_token = response.refresh_token
    som.api_endpoint = response.api_endpoint

    // get new tokens based on your refresh_token (every access_token is valid for 1 hour!)
    som.refresh()
    
    // get a list of students (for pupils this will only be yourself)
    // this also returns your id which can be used to get your grades
    // if you're a pupil this function only returns your own user
    som.students()
      .then(students => {
        som.user_id = students[0].user_id
        console.log(students)
      })
      .catch(error => console.log('Oops, an error occured while fetching students: ' + error))

    // get a list of grades (I currently have none. which means I have no idea if this method works)
    som.grades(1, 5)
      .then(grades => console.log(grades))
      .catch(error => console.log('Oops, an error occured while fetching grades: ' + error))

    // get a list of appointments between 2 dates
    som.schedule('2020-09-21', '2020-09-25')
      .then(appointments => console.log(appointments))
      .catch(error => console.log('Oops, an error occured while fetching schedule: ' + error))

    // get a list of homework assignments on and after the selected date
    som.homework('2020-09-21')
      .then(assignments => console.log(assignments))
      .catch(error => console.log('Oops, an error occured while fetching homework: ' + error))
  })
  .catch(error => console.log('Oops, an error occured during authentication: ' + error))
```

## ToDo

 - [ ] fix som.student(id) method
 - [ ] check som.grades() method
 - [ ] make the documentation better




