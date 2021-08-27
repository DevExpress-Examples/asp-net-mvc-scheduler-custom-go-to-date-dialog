<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128553488/14.1.6%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/E4015)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
<!-- default badges end -->
<!-- default file list -->
*Files to look at*:

* [SchedulerController.cs](./CS/Scheduler.CustomizationGotoDateForm/Controller/SchedulerController.cs)
* [SchedulerExtensionHelper.cs](./CS/Scheduler.CustomizationGotoDateForm/ExtensionHelpers/SchedulerExtensionHelper.cs)
* [SchedulingDataClasses.cs](./CS/Scheduler.CustomizationGotoDateForm/Models/SchedulingDataClasses.cs)
* [Scheduler.cshtml](./CS/Scheduler.CustomizationGotoDateForm/Views/Scheduler/Scheduler.cshtml)
* [SchedulerPartial.cshtml](./CS/Scheduler.CustomizationGotoDateForm/Views/Scheduler/SchedulerPartial.cshtml)
<!-- default file list end -->
# Scheduler - How to customize the Go to Date dialog


<p>This sample project illustrates how you can invoke a custom form instead of the standard <strong>Go to Date </strong>dialog and implement the functionality that enables the end-user to navigate dates using the custom form.<br />
The technique utilized in this example is briefly explained below.</p><p>Let's start with the custom form.<br />
The custom form consists of a single <a href="https://docs.devexpress.com/AspNetMvc/8981/components/data-editors-extensions/calendar"><u>Calendar control</u></a>. Its settings are specified via the <a href="https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.CalendarSettings"><u>CalendarSettings</u></a> properties. <br />
The <a href="https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.MVCxSchedulerOptionsForms.SetGotoDateFormTemplateContent(System.Action-DevExpress.Web.ASPxScheduler.GotoDateFormTemplateContainer-)"><u>SchedulerSettings.OptionsForms.SetGotoDateFormTemplateContent</u></a> method is used to include a rendered custom form into the <strong>SchedulerPartial </strong>view.<br />
To handle date selection within the Calendar control, we subscribe to the corresponding client-side event using the <a href="http://documentation.devexpress.com/#AspNet/DevExpressWebASPxEditorsCalendarClientSideEvents_SelectionChangedtopic"><u>CalendarClientSideEvents.SelectionChanged</u></a> property.<br />
The event handler toggles the value of the boolean <strong>gotoDateCallback </strong>variable that indicates whether a special processing is required on a scheduler callback. Then the client-side <a href="http://documentation.devexpress.com/#AspNet/DevExpressWebASPxSchedulerScriptsASPxClientScheduler_GoToDateFormApplytopic"><strong><u>ASPxClientScheduler.GoToDateFormApply</u></strong></a> method is called that initiates a callback to pass form data to the server control.<br />
To handle a scheduler callback, we subscribe to the <strong>BeginCallback</strong> client-side event of the Scheduler. The <strong>SchedulerSettings.ClientSideEvents.BeginCallback</strong> property specifies the name of the handler function.<br />
If the <strong>gotoDateCallback</strong> indicator is turned on, the function obtains new date from the calendar located in the custom form and passes it on to the callee. <br />
To handle this request properly, we need a specialized <a href="http://documentation.devexpress.com/#AspNet/CustomDocument5462"><u>callback command</u></a>. We inherit this command from the <strong>DevExpress.Web.ASPxScheduler.Internal.GotoDateFormCallbackCommand</strong> class and override its <i>ParseParameters </i>and <i>ExecuteCore </i>methods. To register this command so it is executed instead of the default <i>GotoDateFormCallbackCommand</i>, the <a href="https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.SchedulerSettings.BeforeExecuteCallbackCommand"><strong><u>SchedulerSettings.BeforeExecuteCallbackCommand</u></strong></a><strong> </strong>property is used. It specifies a custom handler that switches command execution when the command with the <strong>SchedulerCallbackCommandId.GotoDateForm </strong>identifier is queried for execution.<br />
Finally, within the <i>ExecuteCore </i>method of a custom <strong>MVCxSchedulerGotoDateFormCallbackCommand</strong> command, methods of the <a href="http://documentation.devexpress.com/#CoreLibraries/clsDevExpressXtraSchedulerServicesIDateTimeNavigationServicetopic"><u>IDateTimeNavigationService interface</u></a> are called to perform date navigation within the scheduler.</p><p>When the application runs, select the <strong>Go to Date</strong> in the context menu. The result is shown in the picture below.</p><p><img src="https://raw.githubusercontent.com/DevExpress-Examples/scheduler-how-to-customize-the-go-to-date-dialog-e4015/14.1.6+/media/d7891d12-4c60-45e5-904b-c62fa5aca5e4.png"></p>

<br/>


