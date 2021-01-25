# Customizing Your Domain on Code Capsules

In this tutorial, we'll go through how to set up a custom domain name for your Code Capsules web site or application.

## Why use a custom domain name?

By default, websites and applications on Code Capsules are accessible from subdomains on `codecapsules.space` (for example: [abesportfolio-wzfg.codecapsules.space](https://abesportfolio-wzfg.codecapsules.space)). These are quite long to type out and can be difficult to remember, so if you're serious about having people visit your website or application, it makes sense to invest in a custom domain name like lincolnportfolio.co.za.

Domain names are memorable and make a website look more professional. Take Google: the domain google.com (or www.google.com) is instantly recognizable. Type the address in your browser's address bar, and you're taken right to Google's search engine. Without a domain, one would need to remember and type in Google's [IP address](https://www.popularmechanics.com/technology/a32729384/how-to-find-ip-address/) address, a string of numbers, in order to visit it.

## How do domains work?

When you type a domain name, such as www.google.com, into your browser's address bar, your browser will send a DNS request to a Domain Name System (DNS) server to find out what IP address the domain you entered corresponds to. There are billions of domain names on the internet, and no DNS server has a complete record of every one, so different DNS servers all over the world communicate with each other to fulfill DNS requests from users.

Different servers have different roles: some are *recursors*, which communicate directly with users, and others are *nameservers*, which recursors receive DNS information from. A single DNS request usually goes through multiple servers before the IP address it's looking for is found. Then this IP address is sent back to your browser, and Google pops up on your screen.

In this lesson we'll learn how to buy a domain and route it to a web application on Code Capsules. Along the way we'll learn more about DNS and related topics.

## Prerequisites

To complete this tutorial, we'll need:

- A web application hosted on [Code Capsules](www.codecapsules.io). If you don't have one yet, try out our [portfolio tutorial](!!!LINK TO PORTFOLIO TUTORIAL!!!)
- A valid payment method (credit card, PayPal, cryptocurrency, bank transfer) to purchase a custom domain.

## Where to buy domain

Accredited businesses that sell domains are called domain registrars. We'll purchase a domain from the registrar Gandi.net. Domains that aren't highly sought after [words with strong commercial potential](https://webhostingcompare.co.za/most-expensive-domain-names-sold-south-africa/) are usually inexpensive. You can also save on domains by using less popular [TLDs](https://en.wikipedia.org/wiki/Top-level_domain) -- for example, registering a `.info` website rather than a `.com` website.

### Purchasing a domain from Gandi

To purchase a domain on [Gandi](www.gandi.net):

1. Navigate to www.gandi.net.
2. Enter the domain you want in the domain search box (for example: lincolnportfolio.co.za)
3. Add the domain to the shopping cart.
4. Checkout by clicking the shopping cart at the top right of the screen.
5. Decide how many years you'd like to pay for now, and then click the **Checkout** button. Domains are renewed annually, but you can save money on a domain you plan to use for a long time by paying for a few years in advance.

Follow the prompts to create an account and purchase your domain. Then log into Gandi.net with your new account and click the **Domain** button on the dashboard.

![image3](images/image3.png)

If Gandi has processed the domain, you'll find it under the **Active** tab. If it is still processing, you'll see it under the **Pending** tab. Domain processing can take some time.

Once your domain is finished processing, it's time to set up HTTPS. 

## Setting up HTTPS for the domain

A domain name is but one part of a URL (Uniform Resource Locator). Google's domain name is www.google.com, and the URL of its homepage is https://www.google.com. Similarly, www.example.com is a domain name, and http://www.example.com is the main URL associated with it.

The HTTP at the front of these URLs stands for Hypertext Transfer Protocol. In the case of Google's URL, the S which follows the HTTP stands for Secure. When you visit http://www.example.com, the website sends data to you in [cleartext](https://www.pcmag.com/encyclopedia/term/cleartext). This means that someone spying on your network traffic can see exactly what data the website is sending you and what data you're sending the website. This is clearly a big problem for a website that deals with sensitive personal or financial information.

While it may not seem like a big deal for a static website like example.com, keep in mind that such a spy (called a [man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)) could also modify this data, and send you anything while making you think it came from example.com.

The solution to these problems is HTTPS, which is HTTP but encrypted. When using an HTTPS site, all data sent between you and the site is encrypted, so a man in the middle cannot easily read or modify it.

HTTPS used to only be used by web applications dealing with highly sensitive information, as it required an expensive annually renewed [X.509 certificate](https://en.wikipedia.org/wiki/Public_key_certificate), but since [Let's Encrypt](https://letsencrypt.org/) launched its free certificate initiative in April 2016, this is no longer the case. As of September 2018, [the majority of the world's top one million websites](https://scotthelme.co.uk/alexa-top-1-million-analysis-august-2018/) use HTTPS. Popular browsers like Google Chrome have recently started marking HTTP websites as not secure.

Gandi provides a web forwarding service that integrates with Let's Encrypt to enable HTTPS for our domain. Let's set that up now:

1. Click on the domain under the active tab.
2. Navigate to the **Web Forwarding** tab.
3. Click on **Create** at the top right.
4. From the address drop-down menu, choose "http:// + https://".
5. Type "www" in the text-box to the right. 
6. From the **Address to forward to** drop-down menu, choose "https://"
7. Type in the name of the domain. 
8. Under **Type of web forwarding**, choose **Permanent**.
9. Your form should look something like the image below. Click **Create** when done.

	![image4](images/image4.png)

10. Repeat this process, but choose "http://" in the **Address** drop-down and type "*" in the textbox next to it.

Now any users who attempt to connect to `http://www.yourdomainhere.tld` or `http://yourdomainhere.tld` will be redirected to the secure URL `https://yourdomainhere.tld`. Gandi handles the creation of your SSL certificate in the background. This can take some time to process. You will need to verify your email address with Gandi before the SSL certificate is created, so check your inbox for an email from Gandi and click the verification link.

## Routing your web application to the domain

Now that the domain has an SSL certificate, we can route it to your Code Capsules web application. Navigate to your domain on the Gandi Dashboard and click on the **DNS Records** tab at the top of the page.

DNS records are how we tell DNS servers which domain names map onto which IP addresses, among other things. There are a number of different kinds of DNS records, but for the purposes of this tutorial, we're only going to look at A and CNAME records.

An A record stores the IP address of the server where your web application is hosted (in this case, Code Capsules). When you type in a domain name, your browser requests the A record associated with the domain from a DNS server. That server contacts other servers to find and then return the A record containing the IP address, and that's what you connect to.

Let's modify the default A record to route to your web application:

1. On [Code Capsules](www.codecapsules.io), navigate to the Capsule you wish to route to your new domain to.
2. Click **Overview**, then **Add A Custom Domain**.
3. Copy the supplied IP address and type in the name of the domain you purchased.
4. Click **Create Domain**.
5. At the DNS record tab in domain view on Gandi, edit the entry with "A" as the type.
6. Enter "@" for its name and paste the Code Capsules supplied IP address in the IPv4 address text box.
7. Click **Create**.

Now your Code Capsules application will be available on your domain, just as our example site is available on https://lincolnportfolio.co.za. However, we had to do some extra work to get https://www.lincolnportfolio.co.za to work, and you'll have to do the same with your domain.

To get this working, we'll add a new CNAME record. A CNAME is like an alias for a domain -- we're going to create one that tells DNS resolvers that it should direct users who enter the leading "www." to the same place as those who leave it out. Here's how to do it:

1. Return to Code Capsules and click **Add A Custom Domain** again.
2. Under domain name, enter "www.yourdomainname.tld", replacing your name and TLD appropriately.
3. Return to the DNS record tab on Gandi, and click on **Add** at the top right.
4. Choose the CNAME type.
5. Enter "www" in the name text-box.
6. Type your Code Capsules web application's default URL under **Hostname**. You can find this in the **Overview** tab of your web application's capsule.
	![image2](images/image2.png)
7. Click **Create**.

You can now view your web application by entering your domain with or without a leading "www". 

## What next?

We've learned how to purchase, secure, and configure a domain, route a domain to your Code Capsules application, and even a little bit about DNS. 

If you're interested, there is still a lot to learn about DNS. A fine place to start is [Amazon Web Services' page on DNS](https://aws.amazon.com/route53/what-is-dns/). Cloudflare also has [a good DNS explainer](https://www.cloudflare.com/learning/dns/what-is-dns/).

If you'd like to know about all the other types of DNS records you can create, in addition to A and CNAME records, this [Google help page](https://support.google.com/a/answer/48090?hl=en) contains a good overview.

Finally, if you'd like to read more about how the HTTP protocol works, [this MDN page](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) is a good place to start.
