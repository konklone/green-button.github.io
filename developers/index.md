---
title: Developers
layout: page
---

## Developers

The Green Button API provides flexible access to Energy Usage Information through a set of RESTful interfaces.

<p>Green Button represents energy usage information as a set of resources as defined in the ESPI standard and uses <a href="http://energyos.github.io/OpenESPI-GreenButton-API-Documentation/API">RESTful APIs</a> to provide
standard access to information for metered resources such as
electricity, gas, and water. These interfaces may be used to access and
manage the metered data by using atom+xml based streams of Energy
Usage Information (EUI) encapsulated within an Atom Feed.</p>

<h3></span></a>RetailCustomer, DataCustodian &amp; ThirdParty Actors</h3>
<p>Green Button allows data to be exchanged between Utilities, Customers, and Third Party Services Providers. It uses standard (http) based messaging to accomplish these exchanges. So, starting with the Green Button Actors:
<dl>
<dt>RetailCustomer</dt>

<dd>Any person or enterprise that is provided services such as
electricity, water, or gas from a resource service provider.
RetailCustomers may be residential, commercial, or industrial.</dd>

<dt>DataCustodian</dt>

<dd>Any enterprise that is holding metered data obtained during the
course of providing resources to a <em>RetailCustomer</em>. A <em>DataCustodian</em>
holds that data as part of the service they provide and may, with the authorization of the
RetailCustomer, allow that data to be shared with a third party.</dd>


<dt>ThirdParty</dt>
<dd>Any person or enterprise that is authorized to have access to metered data held by a <em>DataCustodian</em>.  A <em>ThirdParty</em>, when authorized, may <em>subscribe</em> to a <em>RetailCustomer's</em> data and provide additional services as desired.</dd>
</dl>

<img class="img-responsive" src="{{ site.baseurl }}/assets/GreenButton_Actors_transparent.png" />

<h3>Relationships between the Actors</h3>
<p>The Actors enter into relationships as depicted in the diagram above. The simplest relationship is the one that exists between the DataCustodian (i.e. the Utility) and their customer (the <em>RetailCustomer</em>). This relationship allows the <em>RetailCustomer</em> to download a file that contains their resource usage information. This simple relationship is the basis for the <a href="#download-my-data">Green Button Download My Data</a> operation. 

</div>
<!-- end .home -->

<div id="concepts">
<h3>Concepts</h3>
<p>Green Button uses <a href="http://en.wikipedia.org/wiki/Atom_feed">Atom Publishing Standard</a> to represent structured energy usage information in an XML format that may be exchanged on the internet. Both Google (GData) and Microsoft (OData) independently recognized the power of the Atom Syndication Format to encode complex data for exchange over RESTful web services. Green Button adopted these concepts in the construction of ESPI.</p>

<p>The resources defined within Green Button, UsagePoints, MeterReadings, etc, are expressed, in XML format, within the Atom feed's <a href="#entry">Entry</a> tags. This results in a uniform way to expose full-featured data APIs that reference a Retail Customer's encapsulated Energy Usage Information.</p>

<p>Green Button works by placing data within the <em>&lt;entry&gt;</em> tags
of the Atom stream. Data records are placed within the
<em>&lt;entry&gt; ... &lt;content&gt;</em> tags, and relationships
between tables are represented in the
<em>&lt;link&gt;</em>tags.</p>
<p>
The second thing to note is that in the Atom reprsentation, a feed will always represent a collection of 1 or more Green Button resources:
<pre>
&lt;feed&gt;
  ...
  &lt;entry&gt;
    ...
    &lt;content&gt;
      &lt;espi-resource /&gt;
    &lt;/content&gt;
  &lt;/entry&gt;
  ...
&lt;/feed&gt;
</pre>

<div id="accordion2" class="accordion">
<h3>Relationships</h3>
<div>

<p>So all the <a
 href="https://github.com/energyos/OpenESPI-Common-java/blob/master/etc/espiDerived.xsd">espiDerived.xsd</a>
entities are always contained in a feed and the entry/contents describe the espi entity itself. In
addtion, you need to construct (during the parse if possible for you:) the associations that need to
exist between the espi entities.  The <em>&lt;link&gt;</em> tags are quite important for use during
parsing of the Green Button data. These links allow you to know which <em>MeterReading</em>s are
related to a specific <em>UsagePoint</em>.

<p>Within the
<em>&lt;entry&gt;</em>'s the <em>"related"</em> links point to a collection, for example:
<pre><code>
&lt;feed xmlns="http://www.w3.org/2005/Atom"
         xsi:schemaLocation="http://naesb.org/espi espiDerived.xsd"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"&gt;
 &lt;id&gt;urn:uuid:728B5594-414E-471A-A230-08FCCDAC655C&lt;/id&gt;
 &lt;title&gt;ThirdPartyX Batch Feed&lt;/title&gt;
 &lt;ns3:published&gt;2014-01-02T10:00:00Z&lt;/ns3:published&gt;
 &lt;ns3:updated&gt;2014-01-02T10:00:00Z&lt;/ns3:updated&gt;
 &lt;link rel="self" href="/ThirdParty/83e269c1/Batch"/&gt;
   &lt;entry&gt;
   &lt;id&gt;urn:uuid:97EAEBAD-1214-4A58-A3D4-A16A6DE718E1&lt;/id&gt;
     &lt;link rel="self"
           href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/9b6c7063/UsagePoint/01"/&gt;
     &lt;link rel="up"
           href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/9b6c7063/UsagePoint"/&gt;
     &lt;link rel="related"
           href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/9b6c7063/UsagePoint/01/MeterReading"/&gt;
     &lt;link rel="related"
           href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/9b6c7063/UsagePoint/01/ElectricPowerUsageSummary"/&gt;
     &lt;link rel="related"
           href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/LocalTimeParameters/01"/&gt;
     &lt;title&gt;my house&lt;/title&gt;
     &lt;ns3:published&gt;2014-01-02T10:00:00Z&lt;/ns3:published&gt;
     &lt;ns3:updated&gt;2014-01-02T10:00:00Z&lt;/ns3:updated&gt;
     &lt;content&gt;
       &lt;UsagePoint xmlns="http://naesb.org/espi"&gt;
         &lt;ServiceCategory&gt;
           &lt;kind&gt;0&lt;/kind&gt;
         &lt;/ServiceCategory&gt;
       &lt;/UsagePoint&gt;
     &lt;/content&gt;
   &lt;/entry&gt;
 &lt;/feed&gt;
    </code></pre>
    

  </div>
</div>
<!-- end .concepts -->

<div id="data-elements">
<h3>Green Button Resources</h3>
<p>A <em>DataCustodian</em> will, when authorized by a <em>RetailCustomer</em>, publish a GreenButton data stream. A <em>ThirdParty</em> may then subscribe to that stream. Green Button uses the <a href="http://oauth.net/"><span style="color:blue;">OAuth 2.0 Authorization Framework</span></a> protocol to provide secure authorization for accessing the published data stream.
</p>
<p>Green Button APIs are designed to support data flows that are both large and small. Many Utilities will schedule bulk transfers of hundreds of thousands of 24 hour data as a batch process. In this case, the Green Button APIs must be able to accomodate blocked transfers, recovery, and restarts. Other use-cases are more driven by frequent transmissions of smaller data sets, for example the hourly usage of a single outlet in your home. Green Button is designed to handle both!</p>

<div id="accordion1">
  <h3>ApplicationInformation</h3>
  <div>
    <p></p>
    <pre><code>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="/GreenButtonDataStyleSheet.xslt"?&gt;
&lt;feed xmlns="http://www.w3.org/2005/Atom" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"&gt;
  &lt;id&gt;urn:uuid:f30946a0-0955-466b-901a-366feb2a8424&lt;/id&gt;
  &lt;title&gt;Green Button Usage Feed&lt;/title&gt;
  &lt;updated&gt;2014-05-22T17:45:50Z&lt;/updated&gt;
  &lt;link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ApplicationInformation" rel="self"/&gt;
  &lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
        &lt;ns3:id&gt;urn:uuid:af6e8b03-0299-467e-972a-a883ecdcc575&lt;/ns3:id&gt;
        &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ApplicationInformation" rel="up"/&gt;
        &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ApplicationInformation/1" rel="self"/&gt;
        &lt;ns3:title&gt;GreenButtonData.org  DataCustodian Application&lt;/ns3:title&gt;
        &lt;ns3:content&gt;
              &lt;espi:ApplicationInformation&gt;
                    &lt;espi:dataCustodianApplicationStatus&gt;&lt;/espi:dataCustodianApplicationStatus&gt;
                    &lt;espi:thirdPartyNotifyUri&gt;https://services.greenbuttondata.org/ThirdParty/espi/1_1/Notification&lt;/espi:thirdPartyNotifyUri&gt;
                    &lt;espi:dataCustodianBulkRequestURI&gt;&lt;/espi:dataCustodianBulkRequestURI&gt;
                    &lt;espi:dataCustodianResourceEndpoint&gt;https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource&lt;/espi:dataCustodianResourceEndpoint&gt;
                    &lt;espi:thirdPartyScopeSelectionScreenURI&gt;https://services.greenbuttondata.org/ThirdParty/Subscription/ScopeSelection&lt;/espi:thirdPartyScopeSelectionScreenURI&gt;
                    &lt;espi:client_secret&gt;secret&lt;/espi:client_secret&gt;
                    &lt;espi:redirect_uri&gt;https://services.greenbuttondata.org/ThirdParty/espi/1_1/OAuthCallBack&lt;/espi:redirect_uri&gt;
                    &lt;espi:client_id&gt;third_party&lt;/espi:client_id&gt;
                    &lt;espi:scope&gt;FB=4_5_15;IntervalDuration=900;BlockDuration=monthly;HistoryLength=13&lt;/espi:scope&gt;
                    &lt;espi:scope&gt;FB=4_5_15;IntervalDuration=3600;BlockDuration=monthly;HistoryLength=13&lt;/espi:scope&gt;
                    &lt;espi:dataCustodianId&gt;data_custodian&lt;/espi:dataCustodianId&gt;
                    &lt;espi:thirdPartyApplicationName&gt;Third Party (localhost)&lt;/espi:thirdPartyApplicationName&gt;
              &lt;/espi:ApplicationInformation&gt;
        &lt;/ns3:content&gt;
        &lt;ns3:published&gt;2014-01-02T10:00:00Z&lt;/ns3:published&gt;
        &lt;ns3:updated&gt;2014-01-02T10:00:00Z&lt;/ns3:updated&gt;
  &lt;/ns3:entry&gt;
&lt;/feed&gt;
    </code></pre>
    </div>
  </div>

<div id="accordion">
  <h3>UsagePoint</h3>
  <div>
    <p>A <em>UsagePoint</em> is where a resource is measured. Typically, it is your Utility Smart Meter, but it could be the outlet on the wall as well. UsagePoints provide the reference for all meter readings that are contained within the Green Button data. UsagePoints have a <em>ServiceCategory</em> that defines what <em>kind</em> of resource, such as electricity, gas, or water measurement is being reported. </p>
    <pre><code>&lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
      &lt;ns3:id&gt;urn:uuid:af6e8b03-0299-467e-972a-a883ecdcc575&lt;/ns3:id&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ApplicationInformation" rel="up"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ApplicationInformation/1" rel="self"/&gt;
      &lt;ns3:title&gt;GreenButtonData.org  DataCustodian Application&lt;/ns3:title&gt;
      &lt;ns3:content&gt;
            &lt;espi:ApplicationInformation&gt;
                  &lt;espi:dataCustodianApplicationStatus&gt;&lt;/espi:dataCustodianApplicationStatus&gt;
                  &lt;espi:thirdPartyNotifyUri&gt;https://services.greenbuttondata.org/ThirdParty/espi/1_1/Notification&lt;/espi:thirdPartyNotifyUri&gt;
                  &lt;espi:dataCustodianBulkRequestURI&gt;&lt;/espi:dataCustodianBulkRequestURI&gt;
                  &lt;espi:dataCustodianResourceEndpoint&gt;https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource&lt;/espi:dataCustodianResourceEndpoint&gt;
                  &lt;espi:thirdPartyScopeSelectionScreenURI&gt;https://services.greenbuttondata.org/ThirdParty/Subscription/ScopeSelection&lt;/espi:thirdPartyScopeSelectionScreenURI&gt;
                  &lt;espi:client_secret&gt;secret&lt;/espi:client_secret&gt;
                  &lt;espi:redirect_uri&gt;https://services.greenbuttondata.org/ThirdParty/espi/1_1/OAuthCallBack&lt;/espi:redirect_uri&gt;
                  &lt;espi:client_id&gt;third_party&lt;/espi:client_id&gt;
                  &lt;espi:scope&gt;FB=4_5_15;IntervalDuration=900;BlockDuration=monthly;HistoryLength=13&lt;/espi:scope&gt;
                  &lt;espi:scope&gt;FB=4_5_15;IntervalDuration=3600;BlockDuration=monthly;HistoryLength=13&lt;/espi:scope&gt;
                  &lt;espi:dataCustodianId&gt;data_custodian&lt;/espi:dataCustodianId&gt;
                  &lt;espi:thirdPartyApplicationName&gt;Third Party (localhost)&lt;/espi:thirdPartyApplicationName&gt;
            &lt;/espi:ApplicationInformation&gt;
      &lt;/ns3:content&gt;
      &lt;ns3:published&gt;2014-01-02T10:00:00Z&lt;/ns3:published&gt;
      &lt;ns3:updated&gt;2014-01-02T10:00:00Z&lt;/ns3:updated&gt;
&lt;/ns3:entry&gt;
    </code></pre>
  </div>
  <h3>ReadingType</h3>
  <div>
    <p>A <em>ReadingType</em> provides detail as to the specifics of the reading data that is being obtained. Green Button follows international standards and has the ability to represent large industrial resources as well as those used in a residence.</p>
    <pre><code>&lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
      &lt;ns3:id&gt;urn:uuid:99b292fc-55f7-4f27-a3b9-cddab97cca90&lt;/ns3:id&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ReadingType" rel="up"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ReadingType/1" rel="self"/&gt;
      &lt;ns3:title&gt;Type of Meter Reading Data&lt;/ns3:title&gt;
      &lt;ns3:content&gt;
            &lt;espi:ReadingType&gt;
                  &lt;espi:accumulationBehaviour&gt;4&lt;/espi:accumulationBehaviour&gt;
                  &lt;espi:commodity&gt;1&lt;/espi:commodity&gt;
                  &lt;espi:currency&gt;840&lt;/espi:currency&gt;
                  &lt;espi:dataQualifier&gt;12&lt;/espi:dataQualifier&gt;
                  &lt;espi:flowDirection&gt;1&lt;/espi:flowDirection&gt;
                  &lt;espi:intervalLength&gt;86400&lt;/espi:intervalLength&gt;
                  &lt;espi:kind&gt;12&lt;/espi:kind&gt;
                  &lt;espi:phase&gt;769&lt;/espi:phase&gt;
                  &lt;espi:powerOfTenMultiplier&gt;0&lt;/espi:powerOfTenMultiplier&gt;
                  &lt;espi:timeAttribute&gt;0&lt;/espi:timeAttribute&gt;
                  &lt;espi:uom&gt;72&lt;/espi:uom&gt;
            &lt;/espi:ReadingType&gt;
      &lt;/ns3:content&gt;
      &lt;ns3:published&gt;2013-09-19T04:00:00Z&lt;/ns3:published&gt;
      &lt;ns3:updated&gt;2013-09-19T04:00:00Z&lt;/ns3:updated&gt;
&lt;/ns3:entry&gt;
    </code></pre>
  </div>
  <h3>MeterReading</h3>
  <div>
    <p>A MeterReading is a container for all of the measured <em>IntervalBlocks</em> within the Green Button data captured at a <em>UsagePoint</em>.</p>
    <pre><code>&lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
      &lt;ns3:id&gt;urn:uuid:4234ae39-fb6d-48ca-8856-ac9f41fb3d34&lt;/ns3:id&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/1/UsagePoint/1/MeterReading" rel="up"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/1/UsagePoint/1/MeterReading/1" rel="self"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/1/UsagePoint/1/MeterReading/1/IntervalBlock" rel="related"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/ReadingType/1" rel="related"/&gt;
      &lt;ns3:title&gt;Monthly Electricity Consumption&lt;/ns3:title&gt;
      &lt;ns3:content&gt;
            &lt;espi:MeterReading/&gt;
      &lt;/ns3:content&gt;
      &lt;ns3:published&gt;2013-09-19T04:00:00Z&lt;/ns3:published&gt;
      &lt;ns3:updated&gt;2013-09-19T04:00:00Z&lt;/ns3:updated&gt;
&lt;/ns3:entry&gt;
    </code></pre>
  </div>
  <h3>IntervalBlock</h3>
  <div>
    <p><em>IntervalBlock</em>s are the primary data carrier within the Green Button data. IntervalBlocks may have one or more Intervals, each with a start and duration, as well as the specific <em>IntervalReading</em></p>

    <pre><code>&lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
      &lt;ns3:id&gt;urn:uuid:e0383570-16b1-4ab9-8642-fdb7e89660db&lt;/ns3:id&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/1/UsagePoint/1/MeterReading/1/IntervalBlock" rel="up"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/Subscription/1/UsagePoint/1/MeterReading/1/IntervalBlock/1" rel="self"/&gt;
      &lt;ns3:title&gt;&lt;/ns3:title&gt;
      &lt;ns3:content&gt;
            &lt;espi:IntervalBlock&gt;
                  &lt;espi:interval&gt;
                        &lt;espi:duration&gt;2678400&lt;/espi:duration&gt;
                        &lt;espi:start&gt;1357016400&lt;/espi:start&gt;
                  &lt;/espi:interval&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357016400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357102800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357189200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357275600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357362000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357448400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357534800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357621200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357707600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357794000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357880400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1357966800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358053200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358139600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358226000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358312400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358398800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358485200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358571600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358658000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358744400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358830800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1358917200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359003600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359090000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359176400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;203931&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359262800&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;25662&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359349200&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359435600&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359522000&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
                  &lt;espi:IntervalReading&gt;
                        &lt;espi:cost&gt;256347&lt;/espi:cost&gt;
                        &lt;espi:timePeriod&gt;
                              &lt;espi:duration&gt;86400&lt;/espi:duration&gt;
                              &lt;espi:start&gt;1359608400&lt;/espi:start&gt;
                        &lt;/espi:timePeriod&gt;
                        &lt;espi:value&gt;21021&lt;/espi:value&gt;
                  &lt;/espi:IntervalReading&gt;
            &lt;/espi:IntervalBlock&gt;
      &lt;/ns3:content&gt;
      &lt;ns3:published&gt;2013-02-01T05:00:00Z&lt;/ns3:published&gt;
      &lt;ns3:updated&gt;2013-02-01T05:00:00Z&lt;/ns3:updated&gt;
&lt;/ns3:entry&gt;
    </code></pre>
  </div>
  <h3>LocalTimeParameters</h3>
  <div>
    <p>The <em>LocalTimeParameters</em> provide a flexible manner to enable <em>Energy Usage Information (EUI)</em> to be provided with a reference to local time, without including any <em>Personal Identifiable Information</em>.  </p>
    <pre><code>
&lt;ns3:entry xmlns:espi="http://naesb.org/espi" xmlns:ns3="http://www.w3.org/2005/Atom"&gt;
      &lt;ns3:id&gt;urn:uuid:e30ce77d-ec22-4da5-83c2-991ba34c97d6&lt;/ns3:id&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/LocalTimeParameters" rel="up"/&gt;
      &lt;ns3:link href="https://services.greenbuttondata.org/DataCustodian/espi/1_1/resource/LocalTimeParameters/1" rel="self"/&gt;
      &lt;ns3:title&gt;DST For North America&lt;/ns3:title&gt;
      &lt;ns3:content&gt;
            &lt;espi:LocalTimeParameters&gt;
                  &lt;espi:dstEndRule&gt;B40E2000&lt;/espi:dstEndRule&gt;
                  &lt;espi:dstOffset&gt;3600&lt;/espi:dstOffset&gt;
                  &lt;espi:dstStartRule&gt;360E2000&lt;/espi:dstStartRule&gt;
                  &lt;espi:tzOffset&gt;-18000&lt;/espi:tzOffset&gt;
            &lt;/espi:LocalTimeParameters&gt;
      &lt;/ns3:content&gt;
      &lt;ns3:published&gt;2013-09-19T04:00:00Z&lt;/ns3:published&gt;
      &lt;ns3:updated&gt;2013-09-19T04:00:00Z&lt;/ns3:updated&gt;
&lt;/ns3:entry&gt;
    </code></pre>
  </div>
  <h3>ElectricPowerUsageSummary</h3>
  <div>
    <p></p>
    <pre><code>&lt;entry&gt;
  &lt;id&gt;urn:uuid:97EAEBAD-1214-4A58-A3D4-A16A6DE718E1&lt;/id&gt;
  &lt;published&gt;2012-10-24T00:00:00Z&lt;/published&gt;
  &lt;updated&gt;2012-10-24T00:00:00Z&lt;/updated&gt;
  &lt;link rel="self"
        href="/espi/1_1/resource/Subscription/9b6c7063/ElectricPowerUsageSummary/01"/&gt;
  &lt;link rel="up"
        href="/espi/1_1/resource/Subscription/9b6c7063/ElectricPowerUsageSummary"/&gt;
  &lt;title&gt;my house&lt;/title&gt;
  &lt;content&gt;
    &lt;ElectricPowerUsageSummary xmlns="http://naesb.org/espi"&gt;
      &lt;billingPeriod&gt;
        &lt;duration&gt;2588400&lt;/duration&gt;
        &lt;start&gt;1330578000&lt;/start&gt;
      &lt;/billingPeriod&gt;
      &lt;billLastPeriod&gt;20810000&lt;/billLastPeriod&gt;
      &lt;billToDate&gt;8145000&lt;/billToDate&gt;
      &lt;costAdditionalLastPeriod&gt;4525000&lt;/costAdditionalLastPeriod&gt;
      &lt;currency&gt;840&lt;/currency&gt;
      &lt;overallConsumptionLastPeriod&gt;
        &lt;powerOfTenMultiplier&gt;0&lt;/powerOfTenMultiplier&gt;
        &lt;uom&gt;72&lt;/uom&gt;
        &lt;value&gt;1951364&lt;/value&gt;
      &lt;/overallConsumptionLastPeriod&gt;
      &lt;currentBillingPeriodOverAllConsumption&gt;
        &lt;powerOfTenMultiplier&gt;0&lt;/powerOfTenMultiplier&gt;
        &lt;timeStamp&gt;1334462400&lt;/timeStamp&gt;
        &lt;uom&gt;72&lt;/uom&gt;
        &lt;value&gt;1006640&lt;/value&gt;
      &lt;/currentBillingPeriodOverAllConsumption&gt;
      &lt;qualityOfReading&gt;14&lt;/qualityOfReading&gt;
      &lt;statusTimeStamp&gt;1334462400&lt;/statusTimeStamp&gt;
    &lt;/ElectricPowerUsageSummary&gt;
  &lt;/content&gt;
&lt;/entry&gt;
    </code></pre>
  </div>
  <h3>ElectricPowerQualitySummary</h3>
  <div>
    <p></p>
    <pre><code> &lt;entry&gt;
        &lt;id&gt;urn:uuid:DEB0A337-C1B5-4658-99BA-4688E253A99B&lt;/id&gt;
        &lt;link rel="self" href="Subscription/9b6c7063/ElectricPowerQualitySummary/01"/&gt;
        &lt;link rel="up" type="" href="Subscription/9b6c7063/UsagePoint/01/ElectricPowerQualitySummary"/&gt;
        &lt;title&gt;Quality Summary&lt;/title&gt;
        &lt;content&gt;
            &lt;ElectricPowerQualitySummary xmlns="http://naesb.org/espi"&gt;
                &lt;flickerPlt&gt;1&lt;/flickerPlt&gt;
                &lt;flickerPst&gt;2&lt;/flickerPst&gt;
                &lt;harmonicVoltage&gt;3&lt;/harmonicVoltage&gt;
                &lt;longInterruptions&gt;4&lt;/longInterruptions&gt;
                &lt;mainsVoltage&gt;5&lt;/mainsVoltage&gt;
                &lt;measurementProtocol&gt;6&lt;/measurementProtocol&gt;
                &lt;powerFrequency&gt;7&lt;/powerFrequency&gt;
                &lt;rapidVoltageChanges&gt;8&lt;/rapidVoltageChanges&gt;
                &lt;shortInterruptions&gt;9&lt;/shortInterruptions&gt;
                &lt;summaryInterval&gt;
                    &lt;duration&gt;2119600&lt;/duration&gt;
                    &lt;start&gt;2330578000&lt;/start&gt;
                &lt;/summaryInterval&gt;
                &lt;supplyVoltageDips&gt;10&lt;/supplyVoltageDips&gt;
                &lt;supplyVoltageImbalance&gt;11&lt;/supplyVoltageImbalance&gt;
                &lt;supplyVoltageVariations&gt;12&lt;/supplyVoltageVariations&gt;
                &lt;tempOvervoltage&gt;13&lt;/tempOvervoltage&gt;
            &lt;/ElectricPowerQualitySummary&gt;
        &lt;/content&gt;
        &lt;published&gt;2012-10-24T00:00:00Z&lt;/published&gt;
        &lt;updated&gt;2012-10-24T00:00:00Z&lt;/updated&gt;
    &lt;/entry&gt;
    </code></pre>
  </div>
</div>

</div>
<!-- end .data-elements -->

<div id="examples">
<h3>Samples and Sandboxes</h3>
<dl>
  <dt>Download My Data</dt>
  <dd>A RetailCustomer may download an XML file from either a Data
  Custodian or a Third Party. Restful interfaces are provided to
  enable this operation. There are no assumptions made with respect to
  what the RetailCustomer might do with this XML file, although best
  practices would be to insure the file is viewable by using a minimal
  stylesheet.
<ul>
<li>Examples of <a href="http://services.greenbuttondata.org/sample-data.html">Download My Data files may be found on the Green Button Sandbox</a>.</li>
</ul>
</dd>
  <dt>Connect My Data</dt>

  <dd>The RetailCustomer may also authorize Green Button data to move
  between two machines, such as from their utility to a company that
  might use the data to estimate the energy efficiency of their
  facility. In this case, <em>Connect My Data</em> would be used to
  provide machine-2-machine data transfers. Connect My Data may provide
  a single transfer of information, or transfers on a predefined
  schedule.
<ul>
<li>Connect My Data <a href="http://energyos.github.io/OpenESPI-GreenButton-API-Documentation/API/">RESTful APIs may be experimented with at the API Sandbox</a>.</li>
</ul>
</dd>
</dl>
  </div>
