---
author: George Bantique
date: '2023-03-17T09:04:00+08:00'
excerpt: "Recently Ms. Kate of Reyax Technology reach out with me to introduced their new IoT platform which is the RYC1001 and kind enough to give me a full access to explore this new cloud platform\n \nReyax Technology is a company based in Taiwan, they have a variety of products they even offering IoT solutions according to your project requirements."
guid: https://techtotinker.com/?p=1560
id: 1560
permalink: /?p=1560
title: 'Reyax MQTT with Database: RYC10001 Introduction | Part 1'
url: /reyax-mqtt-with-database-ryc10001-introduction-part-1636-revision-v1-Reyax-MQTT-with-Database-RYC10001-Introduction-Part-1
---


<figure class="wp-block-image size-large is-style-default">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-1-1024x576.png)</figure>Recently Ms. Kate of Reyax Technology reach out with me to introduced their new IoT platform which is the RYC1001 and kind enough to give me a full access to explore this new cloud platform

Reyax Technology is a company based in Taiwan, they have a variety of products they even offering IoT solutions according to your project requirements.

But for today, we will focus on this IoT cloud platform which is the RYC1001.

Firstly, I will use it to demonstrate its basic MQTT protocol then further explore its additional database capability over MQTT protocol later on.

So what is RYC1001?

RYC1001 is built on Amazon Web Services or AWS cloud computing platform which is one of the leaders in cloud technology as of this day.

It supports MQTT protocol which is one of the popular IoT solutions as of now.

It uses a well-thought simple instructions to easily integrate database capability through its MQTT platform via specific topic which we will explore later on.

<div> </div>## INSTRUCTION:

As for the start, lets create the image illustrated above Home automation using basic MQTT.

### 1. Connection Profiles:

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-1-1.png)<figcaption class="wp-element-caption">*I prepared a 2 dashboard, one is to simulate the basic MQTT setup and another one to simulate MQTT setup with RYC1001 database capability.*</figcaption></figure>### 2. **Basic MQTT Setup | Connection:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-2.png)<figcaption class="wp-element-caption">*In the Client ID, I use my username-some identification for me to be able to identify the client.*</figcaption></figure>### 3. **Basic MQTT Setup | Connection continuation:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-3.png)<figcaption class="wp-element-caption">*Username and Password can be acquired from Reyax Technology.*</figcaption></figure>### 4. **Basic MQTT Setup | Dashboard:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-4.png)<figcaption class="wp-element-caption">***lr\_switch** simulates a light switch.*  
***lr\_lights** simulates a light bulb.  
**lr\_temp\_sensor** simulates a temperature sensor.  
**lr\_temperature** simulates a temperature gauge.*</figcaption></figure>### 5. **Basic MQTT Setup | Switch Panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-5.png)<figcaption class="wp-element-caption">*lr\_switch will publish to topic home/living\_room/lights.*</figcaption></figure>### 6. **Basic MQTT Setup | Lights Panel**:

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-6.png)<figcaption class="wp-element-caption">*lr\_lights subscribes to topic: home/living\_room/lights*</figcaption></figure>### 7. **Basic MQTT Setup | Temperature Sensor panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-7.png)<figcaption class="wp-element-caption">*lr\_temp\_sensor publishes to topic: home/living\_room/temperature*</figcaption></figure>### 8. **Basic MQTT Setup | Temperature Gauge panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-8.png)<figcaption class="wp-element-caption">*lr\_temperature subscribes to topic: home/living\_room/temperature.*</figcaption></figure>### **9. MQTT with Database Setup | Connection:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-10.png)</figure>### **10. MQTT with Database Setup | Connection continuation:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-11.png)</figure>### **11. MQTT with Database Setup | Dashboard:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-12.png)</figure>### **12. MQTT with Database Setup | Switch panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-13.png)<figcaption class="wp-element-caption">*lr\_switch needs to publish command message to topic: **api/request** and subscribes to topic: api/command/&lt;Network\_ID&gt;/&lt;deviceTypeId&gt;/&lt;deviceId&gt;/control\_lights or in my case **api/command/35/5/gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic/control\_lights.**  
JsonPath for subscribe: **$.command.result.lights**  
JSON pattern for publish:*  
***{***  
 ***“action”: “command/insert”,***  
 ***“deviceId”: “gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic”,***  
 ***“command”:***  
 ***{***  
 ***“command”: “control\_lights”,***  
 ***“parameters”: {“Control”:”Lights”},***  
 ***“status”:”Done”,***  
 ***“result”: {“lights”: &lt;switch-payload&gt;}***  
 ***}***  
***}***  
&lt;switch-payload&gt; is a variable to send the state of the switch panel.</figcaption></figure>### **13. MQTT with Database Setup | Lights panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-14.png)<figcaption class="wp-element-caption">**lr\_lights subscribes to topic: api/command/&lt;Network\_ID&gt;/&lt;deviceTypeId&gt;/&lt;deviceId&gt;/control\_lights or in my case **api/command/35/5/gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic/control\_lights.**  
JsonPath for subscribe: **$.command.result.lights****</figcaption></figure>### **14. MQTT with Database Setup | Temperature sensor panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-15.png)<figcaption class="wp-element-caption">*lr\_temp\_sensor publishes a notification message to topic: api/request*    
*Subscribe Topic:*  
*api/notification/&lt;Network\_ID&gt;/&lt;deviceTypeId&gt;/&lt;deviceId&gt;/temperature*  
***api/notification/35/5/gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic/temperature***  
*JsonPath for subscribe: **$.notification.parameters.temperature**  
JSON pattern for publish:*    
***{***  
 ***“action”: “notification/insert”,***  
 ***“deviceId”: “gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic”,***  
 ***“notification”:***  
 ***{***  
 ***“notification”: “temperature”,***  
 ***“parameters”: {“temperature”: &lt;slider-payload&gt;}***  
 ***}***  
***}***  
&lt;slider-payload&gt; is a variable that takes the value of slider panel.</figcaption></figure>### **15. MQTT with Database Setup | Temperature gauge panel:**

<figure class="wp-block-image size-full">![](https://techtotinker.com/wp-content/uploads/2023/03/reyax-techtotinker-part2-16.png)<figcaption class="wp-element-caption">lr\_  
lr\_temperature should subscribe to topic:  
*api/notification/&lt;Network\_ID&gt;/&lt;deviceTypeId&gt;/&lt;deviceId&gt;/temperature*  
***api/notification/35/5/gyBII8EdlDdlLXZDfirHAuOiryBIz2RR4zic/temperature***  
*JsonPath for subscribe: **$.notification.parameters.temperature***</figcaption></figure><div> </div>## VIDEO DEMONSTRATION:

<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" frameborder="0" height="281" loading="lazy" src="https://www.youtube.com/embed/tpeTeyjdJ5w?feature=oembed" title="Reyax MQTT with Database: RYC1001 Introduction | Part 1" width="500"></iframe></div></figure><div> </div>## CALL TO ACTION:

For any concern, write your message in the comment section.

You might also like to support my journey on Youtube by Subscribing. [Click this to Subscribe to TechToTinker.](https://www.youtube.com/c/TechToTinker?sub_confirmation=1)

Thank you and have a good days ahead.

See you,

**– George Bantique | tech.to.tinker@gmail.com**

<div> </div>## REFERENCES AND CREDITS:

<div>For more details, you may visit the product details at:</div><div><http://reyax.com/products/ryc1001/></div><div> </div><div>Or if you decided to purchase it, visit:  
<https://www.amazon.com/REYAX-RYC1001-Cloud-Platform-Account/dp/B08M93FTPF>So that’s it, if you enjoy this video please consider supporting my journey in Youtube by Subscribing to TechToTinker Youtube channel.

Thank you.

</div><div> </div><div>References:</div><div>I use JSON Path Finder to find out the exact Json path:  
<https://jsonpathfinder.com/></div><div> </div><div> </div><div> </div><div> </div><div> </div>