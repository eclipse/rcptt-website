---
title: Text Verifications
slug: verifications/text
nav_name: userguide
layout: doc
---
{% import "macros" as m %}

{% include "screenshot-guide" %}
<div class="screenshot">
  <img src="{{site.url}}/shared/img/screenshot-text-verification-editor-1.png"></img>
  
  {# Name #}
  {% set overlayContent %}
  Verification name. This name is automatically synced with file system name. Symbols in name which are not allowed in file names are replaced with 
  underscore character.
  {% endset %}
  {% set top, left, width, height = 93, 118, 100, 19 %}
  {% include "overlay" %}

  {# Tags #}
  {% set overlayContent %}
  Comma-separated list of arbitrary hierarchical tags, i.e. <code>resourcePerspective, jira/PC-1352, windows-only</code>
  {% endset %}
  {% set top, left, width, height = 119, 118, 100, 19 %}
  {% include "overlay" %}
  
  {# Capture button  #}
  {% set overlayContent %}
  Make a snapshot of a text from AUT widget defined in {{m.uiElement("Widget")}} field and copy it into this verification. 
  {% endset %}
  {% set top, left, width, height = 92, 616, 75, 22 %}
  {% include "overlay" %}

  {# Verify button  #}
  {% set overlayContent %}
  Verifies whether a text in a text {{m.uiElement("Widget")}} equals the text from a verification. If {{m.uiElement("Ignore text styling and colors")}} option is off,
  verifies text style as well.  
  {% endset %}
  {% set top, left, width, height = 118, 616, 75, 22 %}
  {% include "overlay" %}
  
  {# Widget #}
  {% set overlayContent %}
  AUT text widget from which a text is taken for verification. You can set the value manually or using {{m.uiElement("Pick...")}} button.
  {% endset %}
  {% set top, left, width, height = 202, 126, 511, 22 %}
  {% include "overlay" %}
  
  {# Pick button #}
  {% set overlayContent %}
  Use this button to select AUT text widget for verification.
  {% endset %}
  {% set top, left, width, height = 202, 643, 42, 22 %}
  {% include "overlay" %}
  
  {# Text area #}
  {% set overlayContent %}
  Verified text. If {{m.uiElement("Ignore text styling and colors")}} option is on you can edit text value. 
  {% endset %}
  {% set top, left, width, height = 261, 75, 595, 159 %}
  {% include "overlay" %}
  
  
 </div>
 
 <h3>How it works</h3>
 
Let's say you need to check the result of creating <i>public static void main</i> method stubs while adding a new Java Class.

So you go through the following steps:

<ul>
<li>Create a new test case, add required <a href = "{{site.url}}/documentation/userguide/contexts">contexts</a> and start recording. 
Create new Java class and check "create public static void main stubs" option. Stop recording - your actions are recorded as ECL script:</li>

<div class="screenshot">
  <img src="{{site.url}}/shared/img/screenshot-text-verification-1.png"></img></div>
  
<li>In a test case editor go to <b>Verification</b> tab and press <span class="uiElement"><img src="{{site.url}}/shared/img/ui-add.gif"></img> Add</span> button.
Click <span class="uiElement"><img src="{{site.url}}/shared/img/ui-capture.gif"></img>Capture</span> button and select <b>Widget text</b> verification type. 
Set verification name and press Finish. Now, while you are in a Capture mode, select Java Class editor. Your verification will be added into a test:</li>


<div class="screenshot">
  <img src="{{site.url}}/shared/img/screenshot-text-verification-2.png"></img></div>

<li>If you open verification you can see that it contains the styled text to verify and the widget to take the text from. Verification checks both: widget 
content and text styles. If you want to verify a plain text only, then you should select {{m.uiElement("Ignore text styling and colors")}} option.</li>

<div class="screenshot">
  <img src="{{site.url}}/shared/img/screenshot-text-verification-3.png"></img></div>
  
<li>Now, when you run your test case, it's verification result will be shown in a Execution View:</li>

<div class="screenshot">
  <img src="{{site.url}}/shared/img/screenshot-text-verification-4.png"></img></div>

</ul>
