# Email Sender Page
This repository contains the **ASP.NET Core** program to send an *email* prefarably Gmail.  
1. Open a new Project on Visual Studio 2019  
	- select the *ASP.NET Core Web Application*  
	- Name the project  
	- choose the *Web Application(Model-View-Controller)*  
2. Add a class named **Email.cs** to the *Models* folder and add the following code in the class  
```     
	public string To { get; set; }
        public string Subject { get; set; }
        public string Body { get; set; }
```  
3. In the *Controllers* folder create a new *MVC Controller(Empty)* and name it **SendMailController.cs**,  
	then write this code replacing your email-id and password
```
	public IActionResult Index()
        {
            return View();
        }
        [HttpPost]
        public IActionResult Index(Email em)
        {
            string to = em.To;
            string subject = em.Subject;
            string body = em.Body;
            MailMessage mm = new MailMessage();
            mm.To.Add(to);
            mm.Subject = subject;
            mm.Body = body;
            mm.From = new MailAddress("your@emailid.com");
            mm.IsBodyHtml = false;
            SmtpClient smtp = new SmtpClient("smtp.gmail.com");
            smtp.Port = 587;
            smtp.UseDefaultCredentials = true;
            smtp.EnableSsl = true;
            smtp.Credentials = new System.Net.NetworkCredential("your@emailid.com", "yourpassword");
            smtp.Send(mm);
            ViewBag.message = "The Email has been sent to '" + em.To + "' Successfully!";
            return View();
        }
```  
4. Double click the *Index* function and add a new *View* with the template as *Create* and Model class as *Email*, you will create **Index.cshtml**, and add this code- 
```
<textarea asp-for="Body" class="form-control" rows="3" cols="15"></textarea>
<h3>@ViewBag.message</h3>
```  
5. In **Startup.cs** replace the *app.UseEndpoints* in *Configure* as
```
app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=SendMail}/{action=Index}/{id?}");
            });
```  
6. Before running the page, go to [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps) and **Allow less secure apps: ON**  
7. Run the page on localhost and check your new email sender!!
