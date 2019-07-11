### clone  가상환경 실행 > pip install -r requirements.txt

[[TIP]]blogapp - models.py[[/TIP]]

```{.python}
    class Post(models.Model):
        title=models.CharField(max_length=200)
        writer = models.CharField(max_length=200)
        pub_date = models.DateTimeField('Date published')
        body = models.TextField()
        user = models.ManyToManyField(User, blank=True)
    
    class Comment(models.Model):
        writer = models.CharField(max_length=200)
        content = models.TextField()
        post = models.ForeignKey(Post, on_delete=models.CASCADE)
```

>blogapp - views.py

```{.python}
def home(request):
    post_list = Post.objects.all()
    if request.user.is_authenticated:
        my_liked_post = Post.objects.filter(user = request.user)
    else:
        my_liked_post = None
    paginator = Paginator(post_list, 3)
    page = request.GET.get('page')
    blogs = paginator.get_page(page)
    return render(request, 'home.html', {'blogs':blogs,'liked_post':my_liked_post})
    
 def like(request,post_id):
    post=get_object_or_404(Post, pk = post_id)
    if post.user.filter(username=request.user.username).exists():
        post.user.remove(request.user)    
    else:
        post.user.add(request.user)
    post.save()
    return redirect('detail', post_id)
 ```
 
 
 > 후 python manage.py makemigrations - python manage.py migrate - python manage.py runserver 하면 완성^^
