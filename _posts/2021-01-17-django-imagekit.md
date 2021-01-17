---
layout: post
title: "django-imageKit 정리"
date: "2021-01-17 23:51:00"
author: Haebin Seo
categories: django django-imagekit
tags: python django django-imagekit
---
# django-imageKit 정리

<h4 id="imagekit">Imagekit<sup><a href="#footnote-1">[1]</a></sup></h4>

Imagekit은 이미지 처리를 위한 Django app이다. resizing이나 cropping같은 작업을 처리하는데 사용된다.
- ImageSpecField class  
  `django.db.models.ImageField`와 매우 유사하게 작동한다. 다만 주어진 옵션대로 자동으로 처리된 이미지를 생성한다는 차이가 있다.
  form에서는 렌더링되지 않는다.
  ```python
  from django.db import models
  from imagekit.models import ImageSpecField
  from imagekit.processors import ResizeToFill

  class Profile(models.Model):
      avatar = models.ImageField(upload_to='avatars')
      avatar_thumbnail = ImageSpecField(source='avatar',
                                        processors=[ResizeToFill(100, 50)],
                                        format='JPEG',
                                        options={'quality': 60})
  
  profile = Profile.objects.all()[0]
  print profile.avatar_thumbnail.url    # > /media/CACHE/images/982d5af84cddddfd0fbf70892b4431e4.jpg
  print profile.avatar_thumbnail.width  # > 100
  ```

- ProcessedImageField class  
  원본이미지가 필요없을 때 사용하면 된다.
  ```python
  from django.db import models
  from imagekit.models import ProcessedImageField

  class Profile(models.Model):
      avatar_thumbnail = ProcessedImageField(upload_to='avatars',
                                            processors=[ResizeToFill(100, 50)],
                                            format='JPEG',
                                            options={'quality': 60})

  profile = Profile.objects.all()[0]
  print profile.avatar_thumbnail.url    # > /media/avatars/MY-avatar.jpg
  print profile.avatar_thumbnail.width  # > 100
  ```

`ProcessedImageField`는 ImageFields 처럼 파일 경로를 데이터베이스에 저장하기 때문에 `upload_to` 인자가 필요하다. 또한 model에 ProcessedImageField를 추가할 시 이를 반영하기 위해 migrate해야 한다.
하지만 `ImageSpecField`는 가상의 field라서 데이터베이스에 저장하지 않는다. 생성된 이미지는 local의 `django.conf.settings.IMAGEKIT_CACHEFILE_DIR`에 저장된다. default는 CACHE/images이다.

- upload_to<sup><a href="#footnote-2">[2]</a></sup><a id="upload-to"></a>  
  파일이 업로드될 경로를 제공한다. 만약 default FileSystemStorage class를 사용한다면 `MEDIA_ROOT` 경로가 앞에 추가된다. upload_to는 함수와 같은 callable이 올 수도 있다. 이 경우에는 `instance`와 `filename`을 인자로 받고, 파일 이름을 포함하는 Unix-style의 업로드 경로를 반환해야 한다.
  
  - instance는 생성된 파일의 인스턴스를 의미하며 파일 경로에 이를 활용할 수 있다.  
    다만 호출 시점에서는 데이터베이스에 저장되기 전이므로 instance.pk는 사용할 수 없다.
  - filename은 확장자를 포함한다.

<br>

#### 참조

<a id="footnote-1" href="#imagekit">[1]</a> Imagekit - [참고](https://gorillaz.tistory.com/10 "https://gorillaz.tistory.com/10"), [document](https://django-imagekit.readthedocs.io/en/latest/index.html "https://django-imagekit.readthedocs.io/en/latest/index.html")

<a id="footnote-2" href="#upload-to">[2]</a> upload_to - [참고](https://tothefullest08.github.io/django/2019/06/04/Django17_image "https://tothefullest08.github.io/django/2019/06/04/Django17_image"), [document](https://docs.djangoproject.com/ko/3.1/ref/models/fields/#django.db.models.FileField.upload_to "https://docs.djangoproject.com/ko/3.1/ref/models/fields/#django.db.models.FileField.upload_to")