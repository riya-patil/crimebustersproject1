---
title: World Crime News Table
layout: default
permalink: /crimebusters/news
---

Stay informed about crime from around the world!

<table>
  <thead>
  <tr>
    <th>News</th>
    <th>News Link</th>
    <th>Source</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- generated rows -->
  </tbody>
</table>

<!--Script is layed out in a sequence (no function) and will execute when page is loaded-->
<script>


  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // prepare fetch options
  const url = "https://world-crime-news1.p.rapidapi.com/news";


const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'cc6d770f58msh120c53d95d27c68p1d2955jsn1898ff4fa031',
		'X-RapidAPI-Host': 'world-crime-news1.p.rapidapi.com'
	}
};

fetch('https://world-crime-news1.p.rapidapi.com/news', options)
	.then(response => response.json())
	.then(response => console.log(response))
	.catch(err => console.error(err));

  // fetch the API
  fetch(url, options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
      }
      // valid response will have json data
      response.json().then(response => {
          console.log(response.data);

          

          // Country data
          for (const row of response.data) {  //change here!
                              // response.data instead of response
            console.log(row);

            // tr for each row
            const tr = document.createElement("tr");
            // td for each column
            const name = document.createElement("td");
            const cases = document.createElement("td");
            const deaths = document.createElement("td");
            const active = document.createElement("td");

            // data is specific to the API
            name.innerHTML = row.title;
            cases.innerHTML = row.url; 
            deaths.innerHTML = row.source; 

            // this build td's into tr
            tr.appendChild(name);
            tr.appendChild(cases);
            tr.appendChild(deaths);

            // add HTML to container
            resultContainer.appendChild(tr);
          }
          
      })
  })
  // catch fetch errors (ie ACCESS to server blocked)
  .catch(err => {
    console.error(err);
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.innerHTML = err;
    tr.appendChild(td);
    resultContainer.appendChild(tr);
  });

</script>