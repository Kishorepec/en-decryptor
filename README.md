Got it! Since you're prepping for your exam, here's a cleaner breakdown of the Hive installation and setup steps that might make it easier to remember (and explain during your exam). I’ll also give you a simple trick at the end to help with memorizing the URLs and commands.

---

### ✅ Apache Hive Installation & Setup (Hive 3.1.2 with Derby DB)

#### **Step 1: Install Hadoop**
Make sure Hadoop is already installed and configured.

#### **Step 2: Download and Extract Hive**
```bash
wget https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
tar -xvzf apache-hive-3.1.2-bin.tar.gz
```

#### **Step 3: Set Hive Environment Variables**
Add to `~/.bashrc`:
```bash
export HIVE_HOME=/home/youruser/apache-hive-3.1.2-bin
export PATH=$PATH:$HIVE_HOME/bin
```
Then:
```bash
source ~/.bashrc
```

#### **Step 4: Update Hadoop Config for Hive (core-site.xml)**
Edit file:
```bash
sudo gedit ~/hadoop/etc/hadoop/core-site.xml
```
Add:
```xml
<property>
  <name>hadoop.proxyuser.<youruser>.groups</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.<youruser>.hosts</name>
  <value>*</value>
</property>
```

#### **Step 5: Set Hive Directories in HDFS**
```bash
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/hive
hdfs dfs -mkdir /user/hive/warehouse
hdfs dfs -mkdir /tmp

hdfs dfs -chmod ugo+rwx /tmp
hdfs dfs -chmod ugo+rwx /user/hive/warehouse
```

#### **Step 6: Setup Hive Config**
Rename the config:
```bash
cd $HIVE_HOME/conf
cp hive-default.xml.template hive-site.xml
```
Remove HTML char `&#8;` from around line 3215 in `hive-site.xml`.

#### **Step 7: Initialize Hive with Derby**
```bash
cd $HIVE_HOME
bin/schematool -dbType derby -initSchema
```

#### **Step 8: Start Hive Services**
- In **Tab 1**:
```bash
hiveserver2
```

- In **Tab 2**:
```bash
cd $HIVE_HOME/bin
beeline -n youruser -u jdbc:hive2://localhost:10000
```

#### **Step 9: Check Hive**
```sql
show databases;
create database Emp;
use Emp;
```

---

### 🧪 CRUD Example (Retail schema)
```sql
create table employee (
  sno int,
  user string,
  city string
)
row format delimited
fields terminated by '\n'
stored as textfile;

show tables;

-- Insert example
insert into employee values (1, 'Alice', 'New York');

-- Read
select * from employee;

-- Update (Hive doesn't support UPDATE traditionally in textfile format)

-- Delete (Not supported in traditional Hive tables with textfiles either)
```

---

### 🧠 Tips to Remember URLs & Commands:
1. **Hive Download URL** → Always starts with:
   ```
   https://downloads.apache.org/hive/hive-<version>/
   ```
   For Hive 3.1.2:
   ```
   https://downloads.apache.org/hive/hive-3.1.2/
   ```

2. **Create a cheat sheet** in your Notes app or Notion:
   - Divide into `Download`, `Setup`, `HDFS`, `Hive Config`, `Init`, and `Commands`.

3. **Practice typing** the commands once by hand (muscle memory helps a LOT).

4. **Associate version with year**: Hive 3.1.2 was released around 2019–2020, just think of Hive as an "older yet stable" version.

---

Want me to generate a one-page PDF cheat sheet for you? I can do that real quick!
