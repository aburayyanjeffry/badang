# Chapter 7
<div style="text-align: center;">
  <img src="/img/chap7-01-cf.png" alt="Description of the image" width="700"/>
</div>
Typically, when we want to expose a service in our home lab, we need to know our home router's public IP and port map the service to the backend computer. The problem is that most home internet connections have a dynamic IP, making it troublesome to have stable access.

Another solution is to use Cloudflare Tunnel. To use this, you need a Cloudflare account and a paid domain from Cloudflare, ranging from USD 7 to USD 20+ per year. That's all there is to it. With that minimal cost, you can serve your service from home! For this chapter, I will share how I expose a wiki on Badang using a Cloudflare Tunnel.



## 1. Ensure the backend servis is running
I'm running a wikimedia on Docker. It is exposing the service at the http://localhost:8080
<div style="text-align: center;">
  <img src="/img/chap7-02-term.png" alt="Description of the image" width="700"/>
</div>

## 1. Set the tunnel at Cloudflare 
Go to `Zero Trust` and just follow through. 
<div style="text-align: center;">
  <img src="/img/chap7-03-cf-tunnel.png" alt="Description of the image" width="700"/>
</div>

## 2. Set the domain and the backend service
Select the desired domain and configure the backend service. Follow the clicks until the end.
<div style="text-align: center;">
  <img src="/img/chap7-04-cf-tunnel.png" alt="Description of the image" width="700"/>
</div>


## 4. Welcome to https://badang.jeffrys.org
If everything is set up correctly, you will be able to access the backend service immediately. Please note that the site is secured with TLS. If you can reach this point, please leave a comment :)
<div style="text-align: center;">
  <img src="/img/chap7-05.png" alt="Description of the image" width="700"/>
</div>
