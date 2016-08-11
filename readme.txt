I have created and written C++ programs using the point clouds library to obtain, compare, and display point clouds. 
I am currently using the ICP algorithm to compare two different point clouds. The program uses ICP to compare two 
different point clouds, and creates a window displaying the input point cloud overlaid with the output point cloud.

Note: Application tested to run on Ubuntu 14.04 64x. 

Drivers:
    If you do not have the necessary drivers for the Kinect, follow the below directions to download and install the correct drivers for Ubuntu 14.04 64x .
    1. Download the drivers from this linK: https://drive.google.com/folderview?id=0B1SpESWn0k3FM2Vfb3hrSHNtYTQ&usp=sharing
    2. Open the directory you have just now downloaded.
    3. There are two directories in the directory you have downloaded. Each of these folders has an install.sh program which will install the necessary drivers. Run the install.sh programs inside both of the directories inside the folder you have just downloaded to install the necessary Kinect drivers or Ubuntu 14.04 64x.

Follow the below instructions to build and run my application:

Build the files
  1. Build pcdNaN.cpp:
    a. run "cp CMakeListsPCDNAN.txt CMakeLists.txt"
    b. run "cd build"
    c. run "cmake ../"
    d. run "make"
    e. run "cd .."
  
   2. Build	openni_grabber.cpp:
    a. run "cp CMakeListsOpenniGrabber.txt CMakeLists.txt"
    b. run "cd build"
    c. run "cmake ../"
    d. run "make"
    e. "cd .."
    
   3. Build iterative_closest_point.cpp:
    a. run "cp CMakeListsIcp.txt CMakeLists.txt"
    b. run "cd build"
    c. run "cmake ../"
    d. run "make"
    
Run the application:
  1. To get our first kinect poiint cloud,run "sudo ./openni_grabber" . A window will pop up when this command that should be showing the point cloud gotten from the Kinect. If this is not the case, press the "R" key until you see a shite point cloud image on a black background. To properly obtain a point cloud from this program, quit the program by pressing the "x" button on the window displaying the Kinect point cloud. There will be a delay between hitting the "x" button on the window and the window closing, but this is alright. This delay is caused by the fact that the program is obtaining a point cloud from the kinect. If you try and quit the program by typing "^C" into the command prompt, the point cloud you obtain will be incomplete and of low caliber.
  2. When the previous command is run, a file named "test_pcd_Binary.pcd" will now be present in the directory where openni_grabber was run. Run the following command to change the file to its correct file name: 
    "sudo mv test_pcd_Binary.pcd room_scan1.pcd"
  3.  To get our second kinect poiint cloud, run "sudo ./openni_grabber" . Ensure that you do not move the Kinect from the postion it was in when you obtained your first point cloud. I would advise you to change the scene in which you are capturing the point cloud (eg. moving a box or other object the was also in the other point cloud) so that when you compare the two point clouds, you will be able to see how the program shows the differences between the two point clouds.  A window will pop up when this command that should be showing the point cloud gotten from the Kinect. If this is not the case, press the "R" key until you see a white point cloud image on a black background. To properly obtain a point cloud from this program, quit the program by pressing the "x" button on the window displaying the Kinect point cloud. There will be a delay between hitting the "x" button on the window and the window closing, but this is alright. If you try and quit the program by typing "^C" into the command prompt, the point cloud you obtain will be incomplete and of low caliber.

  4. Before comparing the raw point clouds from our kinect, it is necessary to remove the NaN's (not a numbers) from the point cloud files. To do this we will use our pcdNaN executable. The command line arguements for using this program are as follows: "sudo ./pcdNaN <input file> <output file>". To clean up our raw kinect point clouds, run the following commands: "sudo ./pcdNaN room_scan1.pcd room_scan1.pcd", "sudo ./pcdNaN room_scan2.pcd room_scan2.pcd"
  5. To now compare these two point cloud, run "sudo ./iterative_closest_point" . It will take a while to complete this process so don't be alarmed if there is not an immediate result given when running the aforementioned command. When the command has executed, a window displaying two point clouds will be displayed. The red cloud is the input cloud, and the black cloud is the transformed input cloud. If you made any changes to the scene between taking the first and second point clouds, it should be clearly visible in this point cloud viewer. Note: There is an example image in this git repository showing the two aforementioned point clouds overlayed with one another.  Now look at your console output. "has converged" tells you if the two point clouds you took line up. If you do not move the kinect between taking point clouds, the value next to "has converged" should be "1", else , if the point clouds do not line up, the value next to "has converged" should be "0". This could be the result of moving the kinect between taking your first and second point clouds. "score" refers to a euclidian fitness score. This score reflects how much of a difference there is between the two point clouds you took. If the euclidian fitness score is near zero, then it means that the two point clouds that you took are extremely similar if not the same. If this score is greater than zero by a good amount (~.00001 in this context), than there is most likely a significant change in the scene. The first three columns represent an identity matrix. The fourth column represents the X, Y, and Z translations used to fit the second point cloud with the first. If your were to compare identical point clouds, it would look something like this:
                         1 0 0 0
                         0 1 0 0
                         0 0 1 0
                        
    In this identity matrix, there are ones going diagonally from the top left corner to the bottom right corner, and all of the other values are zero.
    
    
  FAQ:
      Q: What is an Identity Matrix?
      A: Look Here: https://en.wikipedia.org/wiki/Identity_matrix
      Q: I am seeing graph axis and/or red, green, and black squares. How do I see the point cloud?
      A: Press the "R" key until you see the point cloud
      
      Q: Why am I getting the below errors when I run "./openni_grabber"?
          Warning: USB events thread - failed to set priority. This might cause loss of data...
          terminate called after throwing an instance of 'pcl::IOException'
            what():  : [pcl::PCDWriter::writeBinary] Error during open!
          Aborted (core dumped)

          openni_grabber: /usr/include/boost/smart_ptr/shared_ptr.hpp:646: typename boost::detail::sp_dereference<T>::type boost::shared_ptr<T>::operator*() const [with T = xn::NodeInfo; typename boost::detail::sp_dereference<T>::type = xn::NodeInfo&]: Assertion `px != 0' failed.
          Aborted (core dumped)
      A: Use sudo (eg. "sudo ./openni_grabber")
      
      Q: What to do with these errors: "cp: cannot open 'room_scan1.pcd' for reading: Permission denied" and/or "cp: cannot open 'room_scan2.pcd' for reading: Permission denied" 
      A: Use sudo for the cp command
    
  
