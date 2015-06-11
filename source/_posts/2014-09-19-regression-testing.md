---
title: Regression Testing with RCP Testing Tool
author: Ivan Inozemtsev
categories: [tutorials]
forum: https://www.eclipse.org/forums/index.php/t/852798/
---
{% import "macros" as m %}

This article is a shortened version of an article published for Eclipse Community newsletter ([September 2014](http://www.eclipse.org/community/eclipse_newsletter/2014/september/article3.php)), and it demonstrates how to create UI tests with RCP Testing Tool. Existing Eclipse bugs from Eclipse bugzilla were used as scenarios for test cases. If you are not familiar with RCPTT, please check out our [main page](http://eclipse.org/rcptt) and [Get Started]({{site.url}}/documentation/userguide/getstarted) guide.

### Preparing test development environment
This is straightforward process and takes less than 10 minutes:

1. [Download](http://eclipse.org/rcptt/download), unpack, and launch RCPTT.
2. In a context menu of {{m.uiElement("Test Explorer", "#{site.url}/shared/img/ui-workspace.gif")}} view select <span class="uiElement">New &#8594; <img src="{{site.url}}/shared/img/ui-rcptt-project.png"/> RCP Testing Tool Project</span>.
3. In a context menu of {{m.uiElement("Applications", "#{site.url}/shared/img/ui-applications.gif")}} view select <span class="uiElement"><img src="{{site.url}}/shared/img/ui-new-aut.gif"/> New&#8230;</span> and browse for an application-under-test (I'm going to use Eclipse Luna).</li>
4. Once you add an application-under-test, double-click it in {{m.uiElement("Applications", "#{site.url}/shared/img/ui-applications.gif")}} view to launch.

The final setup should look like this:

<a href="{{site.url}}/shared/img/blog/regression/init.png"><img src="{{site.url}}/shared/img/blog/regression/init.png" width="600"/></a>

That's it. Now you are ready to go. The general workflow for test case design looks like this:

1. Describe an initial state using contexts.
2. Create a test case with required context references.
3. Record actions and assertions.
4. Modify recorded script if necessary.

Now, let's design our first test case.
<!-- break -->

### Eclipse bugzilla <a href="https://bugs.eclipse.org/bugs/show_bug.cgi?id=318826">#318826</a>: Comparing identical files opens editor instead of showing dialog

That is pretty old and not really annoying bug. You can easily reproduce it manually &ndash; just select two files with the same content and compare them with each other:

<a href="{{site.url}}/shared/img/blog/regression/compare-aut.png"><img src="{{site.url}}/shared/img/blog/regression/compare-aut.png" width="600"/></a>.

However how difficult is it to create an automated UI test for this, and how much time would it take? For me it took just 3 minutes and 15 seconds, because I was talking over the phone at the same time =). The process in details:

#### Describing initial state

For initial state of test, assume the following:

* There is a resource project in workspace, named <kbd>project</kbd>, with folders <kbd>folder1</kbd> and <kbd>folder2</kbd>. Each folder contains an empty file <kbd>file.txt</kbd>.
* Project Explorer view is open.

To describe this state, we need to create [workspace]({{site.url}}/documentation/userguide/contexts/workspace) context with required project, and [workbench]({{site.url}}/documentation/userguide/contexts/workbench) context with required view.

You can do the following to create a required workspace context:

1. Right-click project, select <span class="uiElement">New &#8594; <img src="{{site.url}}/shared/img/ui-context.gif"/> Context</span>, select {{m.uiElement("Workspace", "#{site.url}/shared/img/ui-workspace.gif")}} context type, name it like <kbd>workspace for comparison</kbd> and click {{m.uiElement("Finish")}}. Empty workspace context is created and opened in the editor.
2. Go to application-under-test and bring your workspace into desired state manually (i.e. create project, folders and files). 
3. Go back to RCPTT IDE and click {{m.uiElement("Capture", "#{site.url}/shared/img/ui-capture.gif")}} button in context editor &#x2013; all projects from the workspace will be copied from an application-under-test into a context.

The resulting workspace context editor should look like this:

<img src="{{site.url}}/shared/img/blog/regression/compare-workspace.png" alt="Screenshot of workspace context"/>

Now this workspace context contains a copy of a project from application-under-test workspace. When this context is applied, it automatically replaces existing projects from a workspace with it's contents. Test case does not care about the previous state of a workspace &ndash; context guarantees that it will match test's expectations.

Let's create a workbench context. The process of context creation is the same as above, but this time you should select Workbench context type and name it like <kbd>Project Explorer view is open</kbd>. You can capture it from AUT, so it would automatically remember selected perspective, open views and open editors. However, for a current test case you don't care about perspective and editors &#x2013; you just want to ensure that Project Explorer view is open. You can just click a Plus icon in Views section and add project explorer view. Your Workbench context should look like the following:

<img src="{{site.url}}/shared/img/blog/regression/compare-workbench.png" alt="Screenshot of workbench context" />

#### Recording a test case

Right-click a project in {{m.uiElement("Test Explorer", "#{site.url}/shared/img/ui-workspace.gif")}} to create a test case, select <span class="uiElement">New &#8594; <img src="{{site.url}}/shared/img/ui-test.gif"/> Test Case</span>, set test case name as <kbd>318826 Comparing identical resources</kbd> and click {{m.uiElement("Finish")}}. Your new test case will be automatically opened in the editor. Add contexts to a test case in {{m.uiElement("Contexts")}} section of an editor and hit {{m.uiElement("Record", "#{site.url}/shared/img/ui-record.gif")}} button. RCPTT IDE will minimize itself, open <a href="https://www.eclipse.org/rcptt/documentation/userguide/controlpanel/">Control Panel</a>, activate AUT, apply contexts and start recording your actions. </p>

<p>Now you can do the the following actions (control panel will be updated with recorded script immediately):</p>
<ol>
  <li>Select <kbd>folder1</kbd> and <kbd>folder2</kbd> in Project explorer, right-click and hit <span class="uiElement">Compare &#8594; With each other</span>.
  </li>
  <li>Switch to <a href="https://www.eclipse.org/rcptt/documentation/userguide/assertions/">assertion mode</a> as soon as the message dialog appear. Click the label <kbd>There are no differences between selected inputs</kbd>, and specify that you want to assert it's caption:
    <p><img src="{{site.url}}/shared/img/blog/regression/compare-assertion.png" alt="compare-assertion.png" /></p>
  </li>
  <li>Switch back to recording mode and close the message dialog.
  </li>
  <li>Select <kbd>folder1/file</kbd> and <kbd>/file</kbd> in project explorer and compare them with each other again.
  </li>
  <li>Simply close an empty compare editor.
  </li>
</ol>

Recording is done. Go back to Control panel, stop recording, save a test case and click Home icon on toolbar to go back to RCPTT IDE.

Now your test case editor should look like this:

<img src="{{site.url}}/shared/img/blog/regression/compare-testcase.png" alt="test case editor" />

In addition (if you want) you can provide some test case description and specify a link to external resource (i.e. bug tracker).

You can replay this test case right from the editor. It will be executed successfully, however that's not what you really want &ndash; right now test case verifies wrong behavior, caused by a bug in an application. The test case should pass only after the bug will be fixed. Before describing a way to make this test case failing, let's quickly go through recorded script:

{% set snippet %}
with [get-view "Package Explorer" | get-tree] {
    select "project/folder1" "project/folder2"
    get-menu "Compare With/Each Other" | click
}
get-window Compare | get-label "There are no differences between the selected inputs." | get-property caption 
    | equals "There are no differences between the selected inputs." | verify-true
get-window Compare | get-button OK | click
with [get-view "Package Explorer" | get-tree] {
    select "project/folder1/file.txt" "project/folder2/file.txt"
    get-menu "Compare With/Each Other" | click
}
get-editor "Compare ('project/folder1/file.txt' - 'project/folder2/file.txt')" | close 
{% endset %}

{{m.ecl("#{snippet}")}}


The scripting language (ECL) is inspired by TCL and Powershell &ndash; it consists of commands with arguments, connected by pipes and an output of the left command can be passed as an input to the right command. Square brackets ({{m.eclInline("[]")}}) are used to pass an output of a command as an argument to another command. Curly brackets ({{m.eclInline("{}")}}) are used to pass a script as an argument. It allows command to execute this script in some context.
  
Here the script begins with {{m.eclInline("with")}} command. This command takes two arguments &ndash; object and script. This command executes the script, while passing given object as an input to each command in the script. The first four lines can be interpreted as this:

1. Take a Project Explorer view ({{m.eclInline('get-view "Project Explorer"')}}).
2. Find a tree in this view ({{m.eclInline('get-tree')}})
3. With this tree, perform the following actions:
   * Select two elements, denoted by paths ({{m.eclInline('select "project/folder1"...')}}).
   * Click <span class="uiElement">Compare With &#8594; Each Other</span> in context menu ({{m.eclInline('get-menu "Compare With/Each Other" | click')}}).

Next line, {{m.eclInline('get-window Compare | get-label ...')}}, was recorded from an assertion mode. The general pattern here looks like this:

{% set snippet %}
get some UI object | get-property 'propertyName'
    | equals/&#x200b;not-equals 'expectedValue'
    | verify-true/&#x200b;verify-false
{% endset %}

{{m.ecl("#{snippet}")}}
<br/>
In this particular case, we added an assertion for a property, which is used to control location, so in fact there is some repetition here, that does not affect anything, but we could just use {{m.eclInline('get-window Compare | get-label "There are..."')}} as well &ndash; in case when there's no label with this text, the test would fail any way.

Another important thing to notice about this script is that it does not contain any waiting logic &ndash; it waits for background operations automatically (calculation of comparison results in this case).

#### Make test to fail

Now let's make our test case to fail correctly. At first, we don't need a last line ({{m.eclInline('get-editor ... | close')}}), because in case of correct behaviour, there should be no such editor. Instead we want to ensure that there is a window with correct label, so let's replace a last line of this script with this:

{% set snippet %}
get-window Compare | get-label "There are no differences between the selected inputs."
{% endset %}
{{m.ecl("#{snippet}")}}

Now, if you execute the resulting test case, it will fail as expected:

<img src="{{site.url}}/shared/img/blog/regression/compare-execution.png" alt="The window 'Compare' could not be found" />

#### Summary

* Contexts are used to describe an initial state of an application.
* Recorded script can be modified.
* During replay, RCPTT runtime automatically waits for background operations.

Let's take a fresh regression bug for the next test case. It has been introduced in Luna, so we can run test on Kepler and Luna to see that it affects only Luna.

### Eclipse bugzilla <a href="https://bugs.eclipse.org/bugs/show_bug.cgi?id=443671">#443671</a>: Add required bundles option looks completely broken

For this bug we can create a test case, that will be doing the following:

1. Open launch configurations dialog.
2. Select OSGi launch configuration, which includes only <kbd>org.eclipse.ui</kbd> plugin.
3. Press {{m.uiElement("Add Required Bundles")}}.
4. Press {{m.uiElement("Validate Bundles")}}.
5. Make sure that validation message box says that no problems detected.

#### State description

We don't care about workspace for this test case, but instead but we care about current launch configurations. And there is a perfect match for this in RCPTT &#x2013; [Launch]({{site.url}}/documentation/userguide/contexts/launch) contexts. So we go to AUT, create launch configuration, create launch context and capture it. The resulting Launch context looks like this:

<img src="{{site.url}}/shared/img/blog/regression/launch-launch.png" alt="Launch context" />

We are going to use <span class="uiElement">Run &#8594; Run Configurations&#8230;</span> menu, so we need to ensure that this menu item exists. As there are perspectives without {{m.uiElement("Run")}} menu (i.e. {{m.uiElement("Resources")}} perspective), we want to state that this test case requires some perspective, which definitely has {{m.uiElement("Run")}} menu, for instance {{m.uiElement("Plug-in Development")}} perspective. To do this, we create a workbench context, switch AUT to {{m.uiElement("Plug-in Development")}} perspective, and capture workbench context from AUT.

#### Recording actions

As usual, create a test case, add launch and workbench contexts to it, and record test actions &ndash; open {{m.uiElement("Launch configurations")}} dialog, select launch configuration and press required buttons. The resulting script will look like this:

{% set snippet %}
get-menu "Run/Run Configurations..." | click
with [get-window "Run Configurations"] {
  get-tree | select "OSGi Framework/org.eclipse.ui osgi launch"
  get-button "Add Required Bundles" | click
  get-button "Validate Bundles" | click
  get-window Validation | get-button OK | click
}
{% endset %}
{{m.ecl("#{snippet}")}}

We can replay it and obviously the test case will pass, as it does not contain any assertions yet.

#### Asserting results

To assert that validation completed successfully, change the last line to this:

{% set snippet %}
get-window Validation | get-label "No problems were detected."
{% endset %}

{{m.ecl("#{snippet}")}}

Once a test case is replayed again, it correctly fails with a message <kbd>The Label "No problems were detected." could not be found</kbd>. To make sure that the test is correct, run the same test without any modifications on Kepler to see that it passes:

<a href="{{site.url}}/shared/img/blog/regression/launch-kepler-luna.png"><img src="{{site.url}}/shared/img/blog/regression/launch-kepler-luna.png" alt="launch-kepler-luna.png" width="600" /></a>

### Eclipse bugzilla <a href="https://bugs.eclipse.org/bugs/show_bug.cgi?id=394900">#394900</a>: Import existing projects broken by malformed .project file

Again, this issue is easy to reproduce manually, but how hard is it to make an automated test for this?

#### Preparing a state

To prepare a state for this test case, we can use [Folder]({{site.url}}/documentation/userguide/contexts/folder) context. At first, create two resource projects in AUT workspace &#x2013; <kbd>corruptedProject</kbd> and <kbd>goodProject</kbd>, then create a folder in home directory, <kbd>projectsToImport</kbd>, and copy projects from AUT workspace into this folder. Then modify <kbd>.project</kbd> file in <kbd>corruptedProject</kbd> to produce an invalid XML.

Next, go back to RCPTT and create a new Folder context. To specify a folder context root, browse for a <kbd>projectsToImport</kbd> folder and click {{m.uiElement("Capture", "#{site.url}/shared/img/ui-capture.gif")}}: (on a screenshot below I also opened the captured file to demonstrate that the XML is really corrupted)

<img src="{{site.url}}/shared/img/blog/regression/import-folder.png" alt="import-folder.png" />

Similar to workspace context, which stores a state of a workspace, a Folder context can be used to control a state of a file system outside of workspace, which is especially useful for testing of import functionality. Note that Folder context editor was smart enough to record a path to a folder in machine-independent way (<kbd>home://projectsToImport/</kbd>), so that this context can be used on other machines.

For this test case we also need a workspace context, which ensures that workspace is empty. You can just create a new workspace context, name it <kbd>empty workspace</kbd> and there's nothing else to do &ndash; by default workspace contexts have no projecs inside and have an option {{m.uiElement("Clear workspace", "#{site.url}/shared/img/ui-checked.png")}} turned on.

#### Recording actions

Create a new test case, add two contexts into it, and record the following actions:

1. Go to <span class="uiElement">File &#8594; Import&#x2026;</span> menu.
1. Select <span class="uiElement">General &#8594; Existing projects</span>.
1. In import dialog, browse for <kbd>projectsToImport</kbd> folder.
1. Click {{m.uiElement("Refresh")}} button.
1. Switch to assertion mode and assert that a Projects section is empty &ndash; click on a tree and add an assertion for <kbd>itemCount</kbd> property.

The recorded script is below:

{% set snippet %}
get-menu "File/Import..." | click
with [get-window Import] {
    get-tree | select "General/Existing Projects into Workspace"
    get-button "Next &gt;" | click
}

set-dialog-result Folder "/Users/ivaninozemtsev/projectsToImport"
with [get-window Import] {
    get-button "Browse..." | click
    get-button Refresh | click
}
get-window Import | get-tree | get-property itemCount | equals 0 | verify-true
{% endset %}

{{m.ecl(snippet)}}

<br/>
You may notice that there's an interesting command recorded &#x2013; {{m.eclCommand('set-dialog-result')}}. That's how RCPTT handles native dialogs: during recording, it remembers a value, selected by user in a native dialog, and stores it as a {{m.eclInline("set-dialog-result")}} command. During test case execution RCPTT runtime automatically returns stored value to application code instead of opening a native dialog.

However, in order to make a test case fully portable across machines and operating systes, we need to slightly modify this script in order to replace an absolute path with a value, relative to a home folder. This can be achieved by using two ECL commands &#x2013; {{m.eclCommand("get-java-propery")}} and {{m.eclCommand("format")}}:

{% set snippet %}
set-dialog-result Folder [format "%s/projectsToImport" [get-java-property "user.home"]]
{% endset %}
{{m.ecl(snippet)}}

#### Asserting results

Next, we also need to modify the last part of a script, which asserts the results &#x2013; instead of asserting that there are no children, actually we want to assert the correct behavior, i.e. there is one child, <kbd>goodProject</kbd>. This can be done like this:

{% set snippet %}
with [get-window Import | get-tree] {
    select "goodProject.*"
    get-property itemCount | equals 1 | verify-true
}
{% endset %}
{{m.ecl(snippet)}}

Pay attention to this line &ndash; {{m.eclInline('select "goodProject.*"')}}. Here we use a regular expression instead of full text of a tree item (full text also includes an absolute path on a file system). It is possible to use {{m.eclInline("format")}} command again to construct a full text here, but in this case it is faster to simply strip an end text by <code class="language-ecl">.\*</code>.

### Conclusion

Finally, you can run all three tests together and see that all of them are failing with expected messages:

<a href="{{site.url}}/shared/img/blog/regression/final.png"><img src="{{site.url}}/shared/img/blog/regression/final.png" alt="final.png" width="600"/></a>

You can use RCPTT in order to produce independent test cases efficiently and quickly. RCPTT allows to execute them without modifications on different operating systems and different versions of Eclipse platform. Give it a try, and share your opinion!

Here are some useful links to get started:

* <a href="http://eclipse.org/rcptt">RCPTT site</a>
* <a href="http://eclipse.org/rcptt/download">RCPTT downloads</a>
* <a href="http://eclipse.org/rcptt/documentation/userguide/getstarted">Get started guide</a>
* <a href="https://www.eclipse.org/forums/eclipse.rcptt">RCPTT forum</a>

