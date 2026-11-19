<!--

script:  https://cdn.jsdelivr.net/npm/mermaid@11.17.2/dist/mermaid.min.js
script: https://cdn.jsdelivr.net/npm/bootstrap@6.0.0-alpha.1/dist/js/bootstrap.bundle.min.js
link: https://cdn.jsdelivr.net/npm/bootstrap@6.0.0-alpha.1/dist/css/bootstrap.min.css

@onload
window.sendData = async function sendData({url='https://script.google.com/macros/s/AKfycbzoho4vFdv5J2e06WGnIOhRW5O0VPAMeMRK9t55oyiGXMJo8Z0OlSC-hIS2Ju30le3xUA/exec', username, email, course, question, value}) {
   //This script will transmit the user's credentials to the associated Google App Script & Google Sheet
   // TODO - Make sure to add new tabs to the training sheet (See Confluence Page for Link to Google Sheet)
     // <TRAINING_NAME>_SCORES - Tracks when a user correctly answers a question / knowledge check
     // <TRAINING_NAME>_VISITS - Tracks when a user visits a section of the training
   //You can instead create a new Google Sheet for tracking course interaction using the template linked in Confluence, but make sure to update url above to use the new Google Sheet's App Script
   
  //Only transmit data if a valid email has been given
  if(email.endsWith(".edu") || email.endsWith(".gov") || email.startsWith("anon_")){
    // We inject the captured selection into your JSON
    const payload = `{
        "username" : "${username}",
        "email" : "${email}", 
        "course" : "${course}",
        "question" : "${question}",
        "value" : "${value}"
    }`;

    try {
        const response = await fetch(url, {
            method: 'POST',
            mode: 'no-cors',
            headers: {
                'Content-Type': 'application/json',
            },
            body: payload,
        });
        
        // Note: With 'no-cors', we cannot read response.text(), but the request sends.
        console.log("Request sent successfully to Google Sheets");
        send.lia("true")
        return "Submission Successful"; 
    } catch (error) {
        console.error("Error:", error);
        alert(error)
        send.lia("Update Failed", [], false)
        return "Error sending data";
    }
  }
  else{
    console.log("Invalid Email")
  }
}

window.user_name="anon_" + Math.floor(Math.random() * 1000000);
window.user_email=window.user_name + "email.edu"

mermaid.initialize({ startOnLoad: false,  flowchart: { wrappingWidth: 400, subGraphTitleMargin: {"top": 15,"bottom": 0} } });


@end

@mermaid: @mermaid_(@uid,```@0```)

@mermaid_
<script run-once="true" modify="false" style="display:block; background: white">
async function draw () {
    const graphDefinition = `@1`;
    const { svg } = await mermaid.render('graphDiv_@0', graphDefinition);
    send.lia("HTML: "+svg);
    send.lia("LIA: stop")
};

draw()
"LIA: wait"
</script>
@end

@mermaid_eval: @mermaid_eval_(@uid)

@mermaid_eval_
<script>
async function draw () {
    const graphDefinition = `@input`;
    const { svg } = await mermaid.render('graphDiv_@0', graphDefinition);
    console.html(svg);
    send.lia("LIA: stop")
};

draw()
"LIA: wait"
</script>
@end

persistent: true

icon: img/rc_logo.png

version: 0.0.1

-->

# PetaLibrary Quick Start Guide

Welcome to the PetaLibrary! This interactive guide is designed for new Allocation Owners and their team members. It covers everything you need to know to successfully manage and interact with your PetaLibrary storage space.

<div style="width:45%; margin: 15px 2.5%; float:left;">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

**Please enter your Research Computing account information: **

Do you have an existing RC account? 

<script input="radio" value="Select Yes or No" options="YES|NO">
  //If yes, reveal the username option
  if("@input" === "YES"){
    document.getElementById("username").hidden = false;
    "YES"
  }
  else if ("@input" === "NO"){
    document.getElementById("username").hidden = true;
    "NO"
  }
  else{
    "Select Yes or No"
  }
 
</script>

<div id="username" hidden>

Research Computing username: 

<script input="text" placeholder="buff1234" >
  let user_name_temp = "@input"
  if(user_name_temp){
    user_name = user_name_temp
    user_name_temp
  }
  else{
    "Enter username"
  }

</script>
</div>

Institutional email address: 

<script input="email" placeholder="e.g. Ralphie@colorado.edu" >
  let user_email_temp = "@input"

  if(user_email_temp){
    if(user_email_temp.endsWith(".edu") || user_email_temp.endsWith(".gov")){
      user_email = user_email_temp
    }
    else{
      document.getElementById("email_warning").innerHTML="WARNING - Please enter an institutional email (.edu) or government email (.gov)"
    }
    user_email_temp
  }
  else{
    "Enter institutional email"
  }

</script>

<div id="email_warning" style="color:red">

</div>

<br>



<script input="submit" default="Submit" style="display:block; text-align:center;"  >
  if(user_email.endsWith(".edu") || user_email.endsWith(".gov")){
    let currentDate = new Date();
    sendData({username: user_name, email: user_email, course:"PL_QUICKSTART_SCORES", question:"START", value: currentDate.toLocaleString()})
    "Information Saved"
  }
  else{
    "Please enter a valid institutional email"
  }
</script>


</div>

<div style="clear:both"></div>

> [!IMPORTANT] 
> We aim to make our online resources accessible to everyone. If you encounter any barriers in the materials contained in this tutorial, please report them through our [support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

---

## Key Policies

<script hidden>
 let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PL_QUICKSTART_VISITS", question:"KEY_POLICIES", value: currentDate.toLocaleString()})
  "LIA: wait"
</script>

<!--- 
* Flow: key policies review, allocation tiers, common tasks and processes
* Feedback on “Troubleshooting Advice” header:
    * Rename to “Common Tasks and Processes” so users don’t skip it
    * Use action-based headers (e.g. “Restoring Missing Data” not “Missing Data”)


* Topics to include under Common Tasks and Processes:
    * Check status of my allocation (covers cstats)
    * Modify my allocation (increase, decrease, etc.)
    * Snapshots (tied to allocation full scenarios)
    * Collaborator left university: frame as offboarding guidance
* Plan: use Gemini/AI to generate quick blurbs per section to speed up drafting


* Todo - Expand each section a bit, create a few diagrams, and then add in questions. 
--->


Understanding PetaLibrary policies ensures your data remains secure and your team retains seamless access.

* **Acceptable Use** <br> The PetaLibrary allocation owner is responsible for ensuring that all use complies with the [PetaLibrary Terms of Service](https://www.colorado.edu/rc/resources/petalibrary/tos), [CURC User Policies](https://curc.readthedocs.io/en/latest/additional-resources/policies.html), and all relevant university policies.

* **Data Security and Compliance** <br> As outlined in the [Acceptable Data Storage Policy](https://curc.readthedocs.io/en/latest/additional-resources/policies.html#acceptable-data-storage-and-use-on-curc-resources),PetaLibrary allocations may not be used to store any data that is US government Classified nor any Controlled Unclassified Information. Additionally, [FCI data](https://curc.readthedocs.io/en/latest/additional-resources/policies.html#federal-contract-information-fci-and-petalibrary-storage-tiers) can only be stored on an "active" tier allocation. 

* **Backup Responsibility** <br> PetaLibrary is a storage system, but it is not a backup service. The PetaLibrary owner is responsible for maintaining secondary copies of critical, irreplaceable data.

* **Acknowledgements** <br> PetaLibrary is a university supported storage service. Any publications supported by the service must explicitly acknowledge and cite the [University of Colorado Boulder PetaLibrary](https://curc.readthedocs.io/en/latest/getting_started/acknowledge_curc_resources.html#acknowledging-petalibrary).



<div style="display: flex; align-items:center; padding:1em; border-top: dashed 1px; border-bottom: dashed 1px; " >

  <img alt="Read the Docs Logo" src="img/RTD_Logo_Dark.svg" style="width:150px; margin-right:15px; background-color:white; border-radius:5px; padding:5px;"> 

  <p style="margin-bottom:0;" > This tutorial provides a quick overview of CURC's PetaLibrary service. For more detailed information, please see [PetaLibrary's online documentation](https://curc.readthedocs.io/en/latest/petalibrary/index.html) </p>

</div>

### ✏ Knowledge Check

<div style="width:45%; margin: 15px 2.5%; float:left; ">

![Two researchers collaborate in a detailed cartoon data center featuring a central "PETALIBRARY" server. The high-capacity storage hub is smiling, with its side panel clearly showing "PETABYTES 100+ PB" amid other scientific data icons.](img/PetaLibrary_Banner.png)<!-- style="border:solid black 1px; border-radius: 15px;" -->

</div>


<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; float:left; margin: 15px 2.5%;" >

Who is the recommended owner of a PetaLibrary Allocation?

[( )] Lead Graduate Student
[(X)] Primary Investigator (PI)
[( )] Department's SpeedType Manager
[( )] No one, the "owner" is an optional role
<script>
let response = ""

if("@input" === "0" ){
  response = "<b>Not Quite.</b> <br> We recommend assigning Graduate Students and Lab Managers as Technical Contacts, which authorizes them to perform routine operations like modifying permissions or requesting quota increases but leaves final authority of the allocation with its owner. <br> <br>"
} else if ("@input" === "1") {
  response = "<b>Correct!</b> <br> Because only the allocation's owner may close an allocation or transfer ownership, we highly recommend assigning this role to the PI or another full time researcher. <br> <br>"
} else if("@input" === "2"){
  response = "<b>Not Quite.</b> <br> We recommend listing SpeedType managers as a Billing Contact, which ensures they are included in billing communications and authorizes them to handle common billing tasks (e.g. updating the allocation's SpeedType).  <br> <br>"
} else if("@input" === "3"){
  response = "<b>Not Quite.</b> <br> All allocations must have a designated owner. Only the Billing Contact and Technical Contact roles are optional. <br> <br>"
}

document.getElementById("roles_question_responses").innerHTML = response

if ("@input" === "1") {
  let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PL_QUICKSTART_SCORES", question:"ROLES_OWNERSHIP", value: currentDate.toLocaleString()})
  send.lia("true")
} else {
  send.lia("")
}

"LIA: wait"
</script>

<div id="roles_question_responses"></div>

</div>
<div style="clear:both"></div>

---

## Allocation Tier Review

<!--- Note: Convert into a 4 grid chart? --->

PetaLibrary offers different storage tiers tailored to specific research data lifecycles. Knowing which tier you own dictates how you should interact with it.

<div>

</div>

* **Active allocations** are the most performant and are configured for direct compute access from environments like Alpine and Blanca.
* **Archive allocations** are configured to maximize data integrity over performance. They are not accessible from compute nodes, though they can be accessed interactively on login nodes.
* **Active+Archive allocations** provide a means of automatically replicating data across the active and archive PetaLibrary tiers. Rather than purchase an active and archive allocation and synchronizing data between the two, with the active+archive tier CURC will manage the replication for you. This provides an additional layer of data recovery as the active and archive tiers are housed in separate data center on campus. 
* **Archive+DR allocations** expand the archive tier's services by automatically backing up snapshots to an offsite location monthly. Backup and restoration of this data are managed entirely by CURC staff.

<!--- Move to "Transferring Data" --->
> [!TIP] Data Storage Plans
> CURC encourages users to consider petalibrary as one part of their research's data storage plan. <br><br> *For computationally intensive workflows,* consider pairing an active tier allocation with `/scratch` and `$SLURM_SCRATCH`, as discussed in ["How to increase I/O performance"](https://curc.readthedocs.io/en/latest/compute/filesystems.html#how-to-increase-i-o-performance). <br><br> *For archival data,* please remember that all PetaLibrary allocations are considered a single copy of your research data. Irreplaceable data must be backed up. 


---

## Common Data Operations

In this section you will find a set of quick guides that cover common data operations.

### Transferring Data 

* **Globus:** The recommended method for transferring large datasets to and from the PetaLibrary, especially when collaborating across institutions. Refer to the [Globus Documentation](https://curc.readthedocs.io/en/latest/compute/data-transfer.html#globus) for setup instructions.
* **Command-Line Tools (`rsync`, `scp`, `rcp`):** Ideal for smaller files or direct transfers from CURC login nodes.
* **SMB Mount:** Allows mounting your active PetaLibrary directory directly to your desktop or laptop for interactive file management.

> **Automated Data Transfers:** For sensor and lab instrument data collection, configure automated transfers using passwordless SSH key authentication. See the [Automated Transfers Guide](https://curc.readthedocs.io/en/latest/compute/data-transfer.html#automated-scp-and-rsync) for security requirements and implementation steps.

### Sharing Data 

| Sharing Method | Target Audience | Key Requirements & Limitations |
| :--- | :--- | :--- |
| **Globus Guest Collections** | External Collaborators | Secure sharing outside CU Boulder without requiring a full campus account. Easy permission management via Globus web app. |
| **PetaLibrary UNIX Groups** | Internal Collaborators | Requires users to have an active RC Account. Collaborators are added to your allocation's designated UNIX group. |

> ⚠️ **Note on File Permissions:** Standard POSIX file permissions and group ownership (`chgrp`, `chmod`, `umask`) dictate who can read/write shared directories. Ensure shared folders have group write permissions enabled (`g+w`) so lab members can collaborate seamlessly.

### Restoring Data
PetaLibrary maintains automated snapshots of your data in hidden `.snapshot` directories located within your allocation. 
* **Self-Service Restore:** Navigate into the `.snapshot` directory of the affected folder, locate the desired timestamp, and copy the file back to your working directory. *Reminder: Retained snapshots consume a portion of your allocated storage space.*
* **Support Request:** If the required snapshot has rotated out or you need assistance with complex restorations, submit a ticket to the University Support Team (UST).

---

## Common Tasks and Processes

This section provides instructions for handling common PetaLibrary Allocation Tasks. While some tasks can be performed by any member of a PetaLibrary allocation, 

### Monitoring Usage

<article> 

<p class="h3"> df command </p>  

We recommend using the `df` command, also known as "disk free", to check the data storage usage of an allocation. This command can be run from any login node and can be used to check allocations belonging to any PetaLibrary tier. 

``` 
$ df -H /pl/active/<allocation_name>

Filesystem                                 Size  Used Avail Use% Mounted on
pla-enc9:/srv/pl/active/<allocation_name>  4.6T  2.4T  2.2T  52% /pl/active/<allocation_name>

```

</article>

---

<article> 

<p class="h3"> .cstats </p>  

For Active and Active+Archive allocations, the most comprehensive and accurate information on data usage can be found in the `.cstats` file. This file is located in the allocation's base directory and is updated on an hourly basis. If the `.cstats` file ever appears empty, then the allocation is likely full. You can verify this by running the `df` command, as detailed earlier.  

<div class="container">
  <div class="row">
    <div class="col">
     Discuss .cstats file here
    </div>
    <div class="col-8">
      ```
$ cat /pl/active/example_allocation/.cstats
      ```
    </div>
  </div>
  <div class="row">
    <div class="col">
      Report Information
    </div>
    <div class="col-8">
      ```
---------------------------------------------------

Date/Time: 2026-11-18 03:00:06 PM

Allocation Name: pla-enc9/active/example_allocation

Mountpoint: /pl/active/example_allocation

      ```
    </div>
  </div>
  <div class="row">
    <div class="col">
     Overall Data Usage
    </div>
    <div class="col-8">
      ```
---------------------------------------------------
Overall Statistics:

Allocated Space: 5.0T

Available Space: 2.5T

Used Space: 2.6T

	Used by data:		2.6T
	Used by snapshot:	250M

      ```
    </div>
  </div>
  <div class="row">
    <div class="col">
     Overall Data Usage
    </div>
    <div class="col-8">
      ```
---------------------------------------------------
User Statistics:

	Username          	UsedData	UsedObjects
	ralp1234          	235G    	860
	colo0001            15T     	158002
---------------------------------------------------

      ```
    </div>
  </div>
</div>
</article>


### Modifying or Renewing Your Allocation

#### Increasing or Decreasing Allocation Size
Allocation owners or designated technical contacts can request changes to storage quotas as research needs evolve by submitting a request through the CURC portal.

#### Updating SpeedType and Allocation Renewals
* **Fiscal Year Renewals:** Allocations are renewed at the end of each University Fiscal Year (July 1). Invoices are sent to the Owner and Billing Contact. Always verify that billing information and the listed SpeedType are correct prior to renewal.

#### Changing Allocation Ownership
Owners can transfer primary ownership of an allocation to another eligible faculty member or researcher by contacting CURC support.

### Offboarding Collaborators and Managing Access
When a collaborator or student leaves the university, take prompt action to secure data and transition responsibilities:
* Request updates to UNIX group file permissions to revoke access for departed members.
* Transfer ownership of specific files or datasets if another researcher is taking over lead analysis.
* **POI Accounts:** If an external or departed collaborator still requires ongoing access, establish a University Person of Interest (POI) account to maintain authorized access securely.

---

## Conclusion

<script hidden>
 let currentDate = new Date();
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_VISITS", question:"CONCLUSION", value: currentDate.toLocaleString()})
  "LIA: wait"
</script>


**Congratulations! You have completed CURC's PetaLibrary Training!**

If you have specific questions about CURC resources, please fill out our [support request form](https://colorado.service-now.com/req_portal?id=ucb_sc_rc_form).

To learn more about Research Computing, con`sider:

* Reading the [Online Documentation](https://curc.readthedocs.io/en/latest/getting_started/navigating_docs.html).
* Attending a [Workshop Training Session or Consult Hours](https://curc.readthedocs.io/en/latest/getting_started/trainings_and_consults/index.html).

--- 


<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; margin: 0 auto;" >

Please let us know how useful you found this online training: 

  [(2)] Very useful
  [(1)] Somewhat useful
  [(0)] Neutral 
  [(-1)] Not so useful
  [(-2)] Not very useful
<script>
  let choices = @input;
  for (const [key, value] of Object.entries(choices)) {
    if(value === 1){
      sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"REVIEW_SCORE", value: key })
      send.lia("Feedback Sent", [], false)
      break;
    }
  }

</script>

</div>

<br>

<div style="width:40%; border: solid black 1px; padding:10px; border-radius: 15px; margin: 0 auto;" >

Please provide any comments you would like to share with us on this course: 

[[___ ___ ___ ___]]
<script>
  let feedback= `@input`
  feedback = feedback.replace(/(\r\n|\n|\r)/g, "_"); //Google Script wont accept breaks, must replace with "_" to ensure the data is received/saved.
  sendData({username: user_name, email: user_email, course:"PETALIBRARY_SCORES", question:"COMMENTS", value: feedback})
  send.lia("Feedback Sent", [], false)
  "LIA: wait"
</script>


</div>

<div style="clear:both"></div>