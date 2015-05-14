---
title: What does each runtime feature stand for?
slug: runtime-features
nav_name: faq
layout: faq
---
{% import "macros" as m %}
<br>
You can find different Runtime Features in RCPTT Advanced Options. Here you can find the description of the most of them. 
<br><br>

<div class="panel panel-default">
  
  <div class="panel-heading"><b>Logging</b></div>

  <!-- Table -->
  <table class="table">
  <colgroup>
  <col width = 40%> 
 <tr>
    <td>Enable Activity Logging</td>
    <td>expanded activities logging</td>
</tr>
<tr>
    <td>Enable per command image capture</td>
    <td>when enabled, should take a screenshot on each ECL command</td>
</tr>
</colgroup>
</table>
</div>

<div class="panel panel-default">
  
  <div class="panel-heading"><b>Reporting</b></div>

  <!-- Table -->
  <table class="table">
   <colgroup>
  <col width = 40%> 
 <tr>
    <td>Include ignored timers</td>
    <td>if enabled, include in Report all  timerExecs info even those which we are not waiting for.</td>
</tr>
<tr>
    <td>Include wait details</td>
    <td>when enabled, includes wait details into Report</td>
</tr>
<tr>
    <td>Include trace and take screenshot</td>
    <td>if enabled, screenshots made by 'take-screenshot' ECL command are included in Execution Report.  </td>
</tr>
</colgroup>
  </table>
</div>

<div class="panel panel-default">
  
  <div class="panel-heading"><b>Runtime Limits</b></div>

  <!-- Table -->
  <table class="table">
   <colgroup>
  <col width = 40%> 
<tr><td>Test Execution Timeout<td>Timeout for a single test execution in seconds</tr>
<tr><td>Context operation runnable timeout</td> <td>Context applying timeout.</td></tr>
<tr><td>Time RCPTT wait for AUT application to startup</td> <td>How many seconds RCPTT should wait for application startup.</td></tr>
<tr><td>AutoBuild Job Hang Skip timeout</td> <td>how long wait for AutoBuild (usually, when we import resources into AUT workspace)</td></tr>
<tr><td>Log waiting for job if exceed</td> <td>if job takes more then a limit, RCPTT writes to log that it waits for a job</td></tr>
<tr><td>Job Hang Skip timeout</td> <td>Job hang skip timeout in milliseconds. If job is running longer than this time, RCPTT Runtime considers that it is hung and moves to the next command.</td></tr></li>
<tr><td>Enable step mode for job after timeout</td> <td>If job executes more than this time (in milliseconds) and sleeps (i.e. executing Thread.sleep() or Object.wait()), then RCPTT considers that this job is waiting for some user actions and continues to the next command.</td></tr>
<tr><td>Step mode: One step every</td> <td>When job is in sleeping mode (see jobTreatAsSleeingTimeout option), execute commands with given delay (in milliseconds) between commands.</td></tr>
<tr><td>Step mode: Step mode timeout</td> <td>Wait timeout in milliseconds for stepping jobs (see jobTreatAsSleepingTimeout and jobSleepingStepTime)</td></tr>
<tr><td>Wait for delayed jobs with delay less then</td> <td>Max job scheduled delay to be waited for in milliseconds. If job is scheduled with delay less than this value, RCPTT sets delay to 0 and waits for job completion (also see jobNullifyRescheduleMaxValue). Otherwise RCPTT Runtime does not wait for job completion if it is scheduled with a delay greater than this value.</td></tr>
<tr><td>Wait jobs completion after context apply</td><td>If there are any jobs started after context applying, wait for their completion during this time (in milliseconds).</td></tr>
<tr><td>Delayed jobs scheduled with delay 0, if not rescheduled more then</td> <td>If job reschedules itself more than times specified by this parameter, RCPTT stops setting delay to 0 (see jobScheduleDelayedMaxtime parameter).</td></tr>
<tr><td>Timeout for background Debug plugin launch Job</td> <td>Timeout in milliseconds for jobs scheduled from eclipse Debug plugin</td></tr>
<tr><td>Nullify timerExec if time is less then</td> <td>If Display.timerExec is scheduled for the delay less than this value (in milliseconds), set delay to 0.</td></tr>
</colgroup>
  </table>
</div>

