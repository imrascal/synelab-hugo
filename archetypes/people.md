---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
role: master
photo: /images/people/{{ .File.ContentBaseName }}.jpg
role_label: 硕士研究生
date: {{ .Date }}
---

<!-- 头像图片请保存到 static/images/people/ 目录 -->
