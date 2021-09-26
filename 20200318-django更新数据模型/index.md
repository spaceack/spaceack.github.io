# Django更新数据模型


在更新模型的时候遇到问题，网上有说删app下`migrations`目录的，有说要删数据库`django_migrations`表的, 还有的要在数据库中更改字段的。

但是有更好的方法， 如下：

1. ` python manage.py makemigrations app`

2. `python manage.py migrate --fake app`

3. `在app.models 中更新字段`

4. `python manage.py makemigrations app` 在migrations文件夹中添加一个新的文件，并将更新添加到db

5. `python manage.py migrate app`

