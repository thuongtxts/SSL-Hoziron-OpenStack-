SSL-Hoziron-OpenStack
=====================
#Cài SSL trên dashboard

##I.	Yêu cầu hệ thống:

•	Ubuntu Server 14.04

•	Openstack gồm các dự án

o	Compute(kvm)

o	Keystone (identity)

o	Glance ( images)

o	Cinder (block storage)

o	Neutron/ classic networking

##II.	Cài đặt các gói cần thiết

Chạy upgrade hệ thống

    apt-get update

Cài đặt các gói cần thiết: apache2, memcached,libapache2-mod-wsgi openstack-dashboard

    apt-get install -y apache2 memcached libapache2-mod-wsgi openstack-dashboard

Xóa openstack-dashboard-ubuntu-theme  mặc đinh
    apt-get remove -y --purge openstack-dashboard-ubuntu-theme

###III.	Cài đặt apache

Kiểm tra hoạt động của openstack-dashboard

    a2enconf openstack-dashboard

Kích hoạt  HTTPS:

    a2ensite default-ssl
 
    a2enmod ssl

Nếu muốn chuyển hướng từ HTTP sang HTTPS, ta kích hoạt rewrite

    a2enmod rewrite
Chỉnh sử file cấu hình trong apache

    vim /etc/apache2/sites-enabled/000-default.conf

Thêm vào đoạn dữ liệu 

    RewriteEngine On
   RewriteCond %{HTTPS} off
   RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}

Khởi động lại dịch vụ apache
    service apache2 restart

##IV.	Cấu hình horizon

Chỉnh sửa file cấu hình dashboard

    vim /etc/openstack-dashboard/local_settings.py

Để sử dụng SSL ta thêm vào :

    CSRF_COOKIE_SECURE = True
    SESSION_COOKIE_SECURE = True
    USE_SSL = True

Và các biến 

    OPENSTACK_HOST = "127.0.0.1"
    OPENSTACK_KEYSTONE_URL = "http://%s:5000/v2.0" % OPENSTACK_HOST
    OPENSTACK_KEYSTONE_DEFAULT_ROLE = "_member_"

Truy cập trang : http://$IPADDRESS/horizon

Kết quả:

<img src=http://i.imgur.com/3gp45FT.png width="60%" height="60%" border="1">
