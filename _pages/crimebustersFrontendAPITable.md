---
title: Crime Safety Quiz 
layout: page
permalink: /crimebusters/quiz
---

# Crime Safety Quiz (Work In Progress)

Quiz yourself on crime safety!

**What this project does**: This is a quiz that tests players on how much they understand about crime safety. It is in a table that records the number of times each answer choice is selected. The answer choices are connected to the backend using the Spring Framework. The number of times users selected the answer choices are saved in a MYSQL database. After the user selects one of the answer choices, there will also be feedback given on whether the user selected the right or wrong answer choice. 

<br>

<mark>Note:</mark> If the table shows an error of <code>TypeError: Failed to fetch https://crimebusters.nighthawkcoding.ml/api/jokes/</code>, see if using a VPN works :)

<br>


<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Question</th>
    <th>A</th>
    <th>B</th>
    <th>C</th>
    <th>D</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (without a function) and will execute when page is loaded -->
<script>

  // prepare HTML defined "result" container for new output
  const resultContainer = document.getElementById("result");

  // keys for joke reactions
  const HAHA = "haha";
  const BOOHOO = "boohoo";
  const CHOICEC = "choiceC";
  const CHOICED = "choiceD";



  // prepare fetch urls
  // const url = "https://flask.nighthawkcodingsociety.com/api/jokes";
  const url = "https://crimebusters.nighthawkcoding.ml/api/jokes";
  const get_url = url +"/";
  const like_url = url + "/like/";  // haha reaction
  const jeer_url = url + "/jeer/";  // boohoo reaction
  const choiceC_url = url + "/choiceC/";  // choiceC option
  const choiceD_url = url + "/choiceD/";  // choiceC option


  // prepare fetch GET options
  const options = {
    method: 'GET', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
  };
  // prepare fetch PUT options, clones with JS Spread Operator (...)
  const put_options = {...options, method: 'PUT'}; // clones and replaces method

  const answers = ["A. People are hesitant to call out of fear they will be identified by the potential criminal", "B. People take for granted that someone else has already contacted the police", "C. They worry about being embarrassed if their suspicions prove to be unfounded", "D. All of the above", "A. Charge at them and take care of it yourself", "B. Hide somewhere safe and call the police", "C. Give up your belongings and run from your house", "D. Don't do anything", "A. A vehicles moving slowly and without lights, or seemingly repetitive or suspicious", "B. Containing one or more suspicious people observed at an unusual hour.", "C. Vehicles being loaded with valuables in front of closed businesses or residences", "D. All of the above", "A. Run away in the other direction as fast as you can", "B. Give them your belongings and retreat a good distance away", "C. Adamantly refuse to listen to their demands", "D. Slowly back up at a slow pace and negotiate with the criminal"];
  // fetch the API
  fetch(get_url, options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          error('GET API response failure: ' + response.status);
          return;
      }
      // valid response will have JSON data
      response.json().then(data => {
          console.log(data);
          var i = 0;
          for (const row of data) {
            if (i == 0) {
              console.log("i = 0");
            }
            if (i == 1) {
              console.log("i = 1");
            }
            // make "tr element" for each "row of data"
            const tr = document.createElement("tr");
            
            // td for joke cell
            const joke = document.createElement("td");
              joke.innerHTML = row.id + ". " + row.joke + "<br />" + answers[i] + "<br />" + answers[i+1] + "<br />" + answers[i+2] + "<br />" + answers[i+3];  // add fetched data to innerHTML

            // td for haha cell with onclick actions
            const haha = document.createElement("td");
              const haha_but = document.createElement('button');
              haha_but.id = HAHA+row.id   // establishes a HAHA JS id for cell
              haha_but.innerHTML = row.haha;  // add fetched "haha count" to innerHTML
              haha_but.onclick = function () {
                // onclick function call with "like parameters"
                reaction(HAHA, like_url+row.id, haha_but.id);  
                console.log(haha_but.id);
                
                for (let i = 1; i <= 4; i++) {
                  if (haha_but.id == "haha".concat(String(i))) {
                  alert('Incorrect. Try again.');
                  }
                }
                
               
              };
              haha.appendChild(haha_but);  // add "haha button" to haha cell

            // td for boohoo cell with onclick actions
            const boohoo = document.createElement("td");
              const boohoo_but = document.createElement('button');
              boohoo_but.id = BOOHOO+row.id  // establishes a BOOHOO JS id for cell
              boohoo_but.innerHTML = row.boohoo;  // add fetched "boohoo count" to innerHTML
              boohoo_but.onclick = function () {
                // onclick function call with "jeer parameters"
                console.log(boohoo_but.id);

                reaction(BOOHOO, jeer_url+row.id, boohoo_but.id);  

                if (boohoo_but.id == "boohoo1" || boohoo_but.id == "boohoo3") {
                  alert('Incorrect. Try again.');
                }

                if (boohoo_but.id == "boohoo2" || boohoo_but.id == "boohoo4") {
                  alert('Correct!');
                }

              };
              boohoo.appendChild(boohoo_but);  // add "boohoo button" to boohoo cell
             
             const choiceC = document.createElement("td");
              const choiceC_but = document.createElement('button');
              choiceC_but.id = CHOICEC+row.id  // establishes a BOOHOO JS id for cell
              choiceC_but.innerHTML = row.choiceC;  // add fetched "boohoo count" to innerHTML
              choiceC_but.onclick = function () {
                // onclick function call with "jeer parameters"
                console.log(choiceC_but.id);

                reaction(CHOICEC, choiceC_url+row.id, choiceC_but.id); 
                
                for (let i = 1; i <= 4; i++) {
                  if (choiceC_but.id == "choiceC".concat(String(i))) {
                  alert('Incorrect. Try again.');
                  }
                }
                
              };
             choiceC.appendChild(choiceC_but);  // add "boohoo button" to boohoo cell

             const choiceD = document.createElement("td");
              const choiceD_but = document.createElement('button');
              choiceD_but.id = CHOICED+row.id  // establishes a BOOHOO JS id for cell
              choiceD_but.innerHTML = row.choiceD;  // add fetched "boohoo count" to innerHTML
              choiceD_but.onclick = function () {
                // onclick function call with "jeer parameters"
                console.log(choiceD_but.id);

                reaction(CHOICED, choiceD_url+row.id, choiceD_but.id);  
                if (choiceD_but.id == "choiceD1" || choiceD_but.id == "choiceD3") {
                  alert('Correct!');
                } 

                if (choiceD_but.id == "choiceD2" || choiceD_but.id == "choiceD4") {
                  alert('Incorrect. Try again.');
                }
              };
             choiceD.appendChild(choiceD_but);  // add "boohoo button" to boohoo cell


            // this builds ALL td's (cells) into tr (row) element
            tr.appendChild(joke);
            tr.appendChild(haha);
            tr.appendChild(boohoo);
            tr.appendChild(choiceC);
            tr.appendChild(choiceD);



            // this adds all the tr (row) work above to the HTML "result" container
            resultContainer.appendChild(tr);
            i+=4;
          }
      })
  })
  // catch fetch errors (ie Nginx ACCESS to server blocked)
  .catch(err => {
    error(err + " " + get_url);
  });

  // Reaction function to likes or jeers user actions
  function reaction(type, put_url, elemID) {

    // fetch the API
    fetch(put_url, put_options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          error("PUT API response failure: " + response.status)
          return;  // api failure
      }
      // valid response will have JSON data
      response.json().then(data => {
          console.log(data);
          // Likes or Jeers updated/incremented
          if (type === HAHA) // like data element
            document.getElementById(elemID).innerHTML = data.haha;  // fetched haha data assigned to haha Document Object Model (DOM)
          else if (type === BOOHOO) // jeer data element
            document.getElementById(elemID).innerHTML = data.boohoo;  // fetched boohoo data assigned to boohoo Document Object Model (DOM)
          else if (type === CHOICEC) // jeer data element
            document.getElementById(elemID).innerHTML = data.choiceC;  // fetched boohoo data assigned to boohoo Document Object Model (DOM)
          else if (type === CHOICED) // jeer data element
            document.getElementById(elemID).innerHTML = data.choiceD;  // fetched boohoo data assigned to boohoo Document Object Model (DOM)
          else
            error("unknown type: " + type);  // should never occur
      })
    })
    // catch fetch errors (ie Nginx ACCESS to server blocked)
    .catch(err => {
      error(err + " " + put_url);
    });
    
  }

  // Something went wrong with actions or responses
  function error(err) {
    // log as Error in console
    console.error(err);
    // append error to resultContainer
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.innerHTML = err;
    tr.appendChild(td);
    resultContainer.appendChild(tr);

  }

</script>