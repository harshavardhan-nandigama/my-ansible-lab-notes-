

## Inventory

```ini
[mongodb]
mongodb.harshavn24.site
[catalogue]
catalogue.harshavn24.site
[redis]
redis.harshavn24.site
[user]
user.harshavn24.site
[cart]
cart.harshavn24.site
[frontend]
frontend.harshavn24.site
[mysql]
mysql.harshavn24.site
[shipping]
shipping.harshavn24.site
[rabbitmq]
rabbitmq.harshavn24.site
[payment]
payment.harshavn24.site

[all:vars]
ansible_user=ec2-user
ansible_password=DevOps321v
```

### Explanation:

✅ Each component (catalogue, cart, shipping, payment, etc.) is mapped to one EC2 instance DNS name.
✅ `all:vars` defines the SSH user and password used to connect to all servers.
✅ Very good practice to split components like this (microservices architecture).

---

### Why this is useful:

👉 When you run: `-e "component=catalogue"`
👉 It will pick `[catalogue]` group → `catalogue.harshavn24.site`
👉 You don’t need to hardcode hosts in your playbooks.
👉 Easy to **add/remove/replace servers** by only editing `inventory.ini`.


