# ros_envelope

This library provides a wrapper/contatiner class for ROS Messages, with a common interface.

ROS Messages do not inherit from a common base class, and hence do not have a common interface. This has limitations, such as the inability to store differenet types of ROS Message in a single vector.

All ROS Messages have the ability to be published by a ros::Publisher object, after they have been advertised with a templated advertise function. It would be usefull to have an object that promises it contains something that can be published. This would allow the object to be passed arround without the client ever needing to be aware of the thing that it contains.

This is implemented in this library with a templated RosEnvelope class which can contain and advertise the class it has been templated with. All concretions of the RosEnvelope template inherit a RosEnvelopeInterface class, so instatiations of this object have a common interface.

## Example Use

    #A vector of envelopes that can contain different types of Ros Message

    std::vector<RosEnvelopeInterface*> mailBag;

    #In this example the default envelope constructor is used, however each envelope can be constructed with of ros message of type <template argument>.

    mailBag.push( new RosEnvelope<sensor_msgs::Imu> )
    mailBag.push( new RosEnvelope<sensor_msgs::geometry_msgs::WrenchStamped> )

    #ModelUpdator will populate and advertise the Ros messages

    while(ros::ok)
    {

        modelUpdator.updateMessages(mailBag);

        for(int i = 0; i < mailBag.size(); i++)
        {
            RosEnvelopeInterface* env = mailbag.at(i);
            env.publish();
        }

        ros::spinOnce();
    }
