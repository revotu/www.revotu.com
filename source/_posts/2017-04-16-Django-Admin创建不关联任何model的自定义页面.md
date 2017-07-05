---
title: Django Admin创建不关联任何model的自定义页面
date: 2017-04-16 16:04:34
tags: [Django]
permalink: django-admin-create-a-custom-view-without-a-model
---
## templates ##
```Python
custom_view.html
```
## views.py ##
```Python
@staff_member_required
def custom_view(request):
    #. . . create objects of MyModel . . .
    #. . . call their processing methods . . .
    #. . . store in context variable . . . 
    r = render_to_response('admin/myapp/custom_view.html', context, RequestContext(request))
    return HttpResponse(r)
```
<!-- more -->
## urls.py ##
```Python
from myapp.views import custom_view
urlpatterns = [
    url(r'^admin/custom_link/$', custom_view, name='custom_name'),
    url(r'^$', RedirectView.as_view(url='/admin/')),
    url(r'^admin/', admin.site.urls),
]
```