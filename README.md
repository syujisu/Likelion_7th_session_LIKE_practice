### clone  가상환경 실행 > pip install -r requirements.txt


>blogapp - models.py

```{.python}
    class Post(models.Model):
    title=models.CharField(max_length=200)
    writer = models.CharField(max_length=200)
    pub_date = models.DateTimeField('Date published')
    body = models.TextField()
    user = models.ManyToManyField(User, blank=True)
```
