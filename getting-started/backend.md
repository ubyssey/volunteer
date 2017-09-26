## Backend Development

### What is backend development?

Web development is often split into two categories: backend and frontend. It's easy for beginners to get confused between the two, especially because there is often some overlap.

Backend development involves writing code and processes that run only on a web server, whereas frontend code runs in the user's web browser on their computer or mobile device. 

Frontend code is visible to everybody and includes a website's HTML, CSS and JavaScript resources, while backend code is almost always hidden from the user. Some examples of backend tasks include:

* Fetching and preparing data from a database
* Authenticating a user when they log into a secure system
* Performing complex calculations or CPU-intensive tasks

Due to security and performance reasons, it's easy to see why these tasks shouldn't happen on the user's device.

### What does a newspaper backend look like?

The backend code for ubyssey.ca is mainly concerned with fetching data from a database \(usually news articles\), preparing the data and sending it back to the user. This process looks something like this:

1. A user sends a request to a URL \(e.g. https://www.ubyssey.ca/news/max-holmes-wins-vp-academic/\)
2. The web server receives the request and parses any relevant information \(e.g. the article slug "max-holmes-wins-vp-academic"\)
3. The server then loads the article data from the database
4. After loading the data, it is converted into an HTML document that is returned to the user's web browser

The rest of this tutorial will be covering the above steps in more detail.

### Introducing Python

The backend code for ubyssey.ca is written in the Python programming language. We choose to use Python for several reasons, the main one being that its fairly easy to learn and many people are already familiar with it. More specifically, our platform is built on top of the popular [Django web framework](https://www.djangoproject.com/), so much of the heavy lifting \(routing, authentication, database access\) is already handled for us.

If you want to learn more about Python, check out the [resources](/resources.md) page. However, this tutorial is designed for beginners who have never used Python before.

### 1. URL Routing

A Universal Resource Locator \(URL\), often called a web address, is what a web browsers uses to access resources stored on a web server. Think of it like a mailing address for a web page. The first part of a URL, called the hostname \(e.g. ww.ubyssey.ca\), is  converted to an IP address \(e.g. 216.239.32.21\) through something called a DNS lookup, which tells the browser which server to send the request to. The server is then responsible for "routing" the remaining portion of the URL \(e.g. /news/max-holmes-wins-vp-academic/\), which is often the first task that a web backend must take care of.

A web server should be able to understand the structure of a URL and send incoming requests to the right place. If the server can't understand the URL or if the URL is invalid, a proper error message should be returned. 

Luckily for us, Django has a set of tools that make this process relatively straightforward. We simply tell Django the structure of a URL and it will route incoming requests to functions that we define. For this tutorial, we'll be focusing on the homepage, which happens to have the simplest URL pattern:

```py
# ubyssey/urls.py

from django.conf.urls import url

theme = UbysseyTheme()

urlpatterns = [
    url(r'^$', theme.home, name='home'),
]
```

URL patterns are defined using [regular expressions](https://en.wikipedia.org/wiki/Regular_expression), which is a way to match multiple strings against a defined structure. For example, here's the pattern for article URLs, which always contain a **section name**, followed by a slash, and then the **article slug:**

```py
url(r'^(?P<section>[-\w]+)/(?P<slug>[-\w]+)/$', theme.article, name='article')

'/news/max-holmes-wins-vp-academic/' # Match
'/sports/fall-softball-classic-2017/' # Match
'/this-is-not-an-article/' # Not a match!
```

You'll notice that Django's `url` helper takes a second parameter, which is the method to send the request to. In the homepage example, if the URL matches an empty string, the request is routed to `theme.home` . The definition for that method lives in [ubyssey/views/main.py](https://github.com/ubyssey/ubyssey.ca/blob/develop/ubyssey/views/main.py#L28):

```py
def home(self, request):
        frontpage = ArticleHelper.get_frontpage(
            sections=('news', 'culture', 'opinion', 'sports', 'features', 'science'),
            max_days=7
        )

        elections = ArticleHelper.get_topic('AMS Elections').order_by('-published_at')

        frontpage_ids = [int(a.id) for a in frontpage[:2]]

        sections = ArticleHelper.get_frontpage_sections(exclude=frontpage_ids)

        try:
            articles = {
                'primary': frontpage[0],
                'secondary': frontpage[1],
                'thumbs': frontpage[2:4],
                'bullets': frontpage[4:6],

             }
        except IndexError:
            raise Exception('Not enough articles to populate the frontpage!')

        popular = ArticleHelper.get_popular()[:5]

        blog = ArticleHelper.get_frontpage(section='blog', limit=5)

        title = '%s - UBC\'s official student newspaper' % self.SITE_TITLE

        context = {
            'title': title,
            'meta': {
                'title': title,
                'description': 'Weekly student newspaper of the University of British Columbia.',
                'url': self.SITE_URL
            },
            'articles': articles,
            'sections': sections,
            'popular': popular,
            'blog': blog,
            'day_of_week': datetime.now().weekday(),
        }
        
        return render(request, 'homepage/base.html', context)
```

For those of you new to Python, the above code might look a little daunting, but don't worry. 

