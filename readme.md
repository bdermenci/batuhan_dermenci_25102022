# Composiv_tryouts for case study

This project is a case study for Eteration application process.

## Description

This project was made as a learning practice for basic ROS concepts such as ROS Nodes, publishing, subscribing and launching mechanism. The goal was to make:
- a simple publisher node (``composiv_talker.py``) that sends messages to a certain topic.
- a simple subscriber node (``composiv_listener.py``) that receives messages from the same topic
- a ``composiv_tryouts.launch`` file that runs previously mentioned nodes from a single point.


## 1. Composiv_talker.py

This node consists of a Python script. The script is shown below.

```python
#!/usr/bin/env python
# license removed for brevity
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```
In this script,
- ``pub = rospy.Publisher('chatter', ...)`` is declaring the publishing topic "chatter".
- ``rospy.init_node('talker', ...)`` is declaring to rospy that the name of the publisher node is "talker".
- ``rate = rospy.Rate(10) # 10hz`` is used for looping the message the publisher node publishing at 10 Hz.
- in the ``while not rospy.is.shutdown():`` loop, a "hello world" string is published to the "chatter" topic.

## 2. Composiv_listener.py

This node also consists of a Python script. It is fairly similar to the publisher node. The script is shown below.

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)
    
def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.
    rospy.init_node('listener', anonymous=True)

    rospy.Subscriber("chatter", String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    listener()
```
In this script,
- ``rospy.Subscriber("chatter", ...)`` declares that your subscriber node is subscribing to the "chatter" topic. 
- ``rospy.init_node('listener', ...)`` is declaring to rospy that the name of the subscriber node is "listener".
- ``rospy.spin()`` keeps the node from exiting until it is shutdown.

## 3. References
- Basic publisher and subscriber node scripts and all other ROS-related information is studied from the [wiki.ros](https://wiki.ros.org/ROS/Tutorials/) tutorials section.
- Markdown syntax is referenced from the [markdownguide](https://www.markdownguide.org/basic-syntax/).
- Git related information is referenced from [https://www.digitalocean.com/community/tutorials/how-to-use-git-effectively](https://www.digitalocean.com/community/tutorials/how-to-use-git-effectively).
