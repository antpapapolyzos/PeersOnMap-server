# PeersOnMap

Το PeersOnMap είναι μια εφαρμογή για Android κινητές συσκευές, που βασίζεται στις συνεισφορές των χρηστών με σκοπό τη δημιουργία ενός χάρτη περισσότερρο φιλικού για πεζούς και ποδηλάτες. Η εφαρμογή προτρέπει τους χρήστες με τη βοήθεια της παιχνιδοποίησης, να ανεβάσουν τα μονοπάτια τους όσο περπατούν ή κάνουν ποδήλατο. Η Ομότιμη Αξιολόγηση (Peer Review) αποτελεί βασικό εργαλείο της εφαρμογής PeersOnMap και βοηθά στη βελτίωση των παραγόμενων χαρτών. Οι χρήστες έχουν τη δυνατότητα μέσα από τη φυσική παρουσία τους σε μια περιοχή, να αξιολογήσουν ή να σχεδιάσουν διορθώσεις για καταγεγραμμένα μονοπάτια που εμφανίζονται στο χάρτη. Ο τελικός στόχος είναι να δημιουργηθεί ένας εναλλακτικός αστικός χάρτης για πεζούς και ποδηλάτες.<br/>
Σημείο έναρξης του συστήματος είναι το σύστημα [PathsOnMap] (https://github.com/ippokratis1/PathsOnMap-Corfu-server.git)

Εδώ παρουσιάζεται ο server της εφαρμογής.<br/>
Βασικές λειτουργίες του είναι:
- η αποθήκευση των αρχείων των διαδρομών που καταγράφουν οι χρήστες στον φάκελο με όνομα upload.
- η εξομάλυνση των διαδρομών που καταγράφουν οι χρήστες και η αποθήκευση των αρχείων που προκύπτουν. 
- η αποθήκευση στη ΒΔ των αξιολογήσεων των χρηστών.
- η αποθήκευση στη ΒΔ των σχεδιάσεων διόρθωσης μονοπατιών από τους χρήστες.
- η εμφάνιση του χάρτη για πεζούς και για ποδηλάτες που έχει δημιουργηθεί από τις συνεισφορές των χρηστών.
- η αποθήκευση στη ΒΔ του προφίλ των χρηστών που συνδέονται στην εφαρμογή.


## Τεχνολογίες και εργαλεία

 - Apache
 - PHP
 - MySQL
 - HTML 5
 - CSS 3
 - JavaScript
 - JQuery 
 - Google Maps JavaScript API 

Τη στιγμή συγγραφής αυτού του οδηγού, ο server εκτελείται στο: https://peersonmap.herokuapp.com/<br/>
![webpage](https://user-images.githubusercontent.com/8918882/30720326-a5ab40fc-9f2f-11e7-9f94-0376bfef9078.png)


## Κατασκευή του κώδικα τοπικά στον υπολογιστή σας

 1. Εγκατάσταση [WampServer](http://www.wampserver.com/en/)
 2. Τρέξτε τον WampServer και επιλέξτε Put Online
 3. Καταβάστε (download zip) τον κώδικα από [εδώ] 
 4. Αποσυμπιέστε τον φάκελο
 6. Kάντε αποκοπή επικόληση του φακέλου στον φάκελο www του WampServer που βρίσκεται στην διαδρομή C:\wamp\www ή C:\wamp64\www
 7. Ανοίξτε ένα browser και πηγαίνετε στην διεύθυνση: localhost. Από την σελίδα που θα εμφανιστεί επιλέξτε phpmyadmin
 8. Δημιουργήστε μια νέα βάση δεδομένων επιλέγοντας Νέα και στην συνέχεια γράψτε για όνομα της βάσης δεδομένων: pom_db και για σύνθεση: utf8_general_ci και κάντε κλικ στο κουμπί *Δημιουργία*
 9. Επιλέξτε την βάση που δημιουργήσατε και πατήστε στην ετικέτα *Δικαιώματα*. Στην συνέχεια επιλέξτε *Προσθήκη χρήστη*. Από εκεί γράψτε για όνομα χρήστη: pom_apap, για φιλοξενητή: localhost και για κωδικό πρόσβασης: !apap1324. Επιλέξτε από Βάση δεδομένων για χρήστη: πλήρη διακιώματα και από Γενικά δικαιώματα: Επιλογή όλων. Μετά πατήστε στο κουμπί: *Εκτέλεση*
 10. Επιλέξτε την ετικέτα Κώδικας SQL και τρέξτε τα παρακάτω ερωτήματα SQL: 


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `players`
>--
>
>DROP TABLE IF EXISTS `players`;
>CREATE TABLE IF NOT EXISTS `players` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `name` varchar(50) NOT NULL,
>  `email` varchar(100) NOT NULL,
>  `encrypted_password` varchar(80) NOT NULL,
>  `salt` varchar(10) NOT NULL,
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  `path_set` int(5) NOT NULL DEFAULT '1',
>  PRIMARY KEY (`uid`),
>  UNIQUE KEY `email` (`email`)
>) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `paths`
>--
>
>DROP TABLE IF EXISTS `paths`;
>CREATE TABLE IF NOT EXISTS `paths` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `player_id` int(11) NOT NULL,
>  `path_raw_google_gpx` varchar(100) NOT NULL,
>  `path_smooth_google_gpx` varchar(100) NOT NULL,
>  `tags` int(11) NOT NULL,
>  `meters` int(11) NOT NULL,
>  `new_path` tinyint(1) DEFAULT '1',
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=InnoDB AUTO_INCREMENT=69 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `pathpoints`
>--
>
>DROP TABLE IF EXISTS `pathpoints`;
>CREATE TABLE IF NOT EXISTS `pathpoints` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `path_id` int(11) NOT NULL,
>  `start_lat` float(10,6) NOT NULL,
>  `start_long` float(10,6) NOT NULL,
>  `middle_lat` float(10,6) NOT NULL,
>  `middle_long` float(10,6) NOT NULL,
>  `end_lat` float(10,6) NOT NULL,
>  `end_long` float(10,6) NOT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `reviews`
>--
>
>DROP TABLE IF EXISTS `reviews`;
>CREATE TABLE IF NOT EXISTS `reviews` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `player_id` int(11) NOT NULL,
>  `path_id` int(11) NOT NULL,
>  `rated` int(1) NOT NULL,
>  `rated_tags` int(1) NOT NULL,
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=InnoDB AUTO_INCREMENT=36 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `suggestedpaths`
>--
>
>DROP TABLE IF EXISTS `suggestedpaths`;
>CREATE TABLE IF NOT EXISTS `suggestedpaths` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `path_id` int(11) NOT NULL,
>  `player_id` int(11) NOT NULL,
>  `start_lat` float(10,6) NOT NULL,
>  `start_long` float(10,6) NOT NULL,
>  `end_lat` float(10,6) NOT NULL,
>  `end_long` float(10,6) NOT NULL,
>  `pending` tinyint(1) NOT NULL DEFAULT '1',
>  `rate` int(5) NOT NULL DEFAULT '3',
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=18 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `cyclepaths`
>--
>
>DROP TABLE IF EXISTS `cyclepaths`;
>CREATE TABLE IF NOT EXISTS `cyclepaths` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `player_id` int(11) NOT NULL,
>  `path_raw_gpx` varchar(100) NOT NULL,
>  `path_smooth_gpx` varchar(100) NOT NULL,
>  `tags` int(11) NOT NULL,
>  `meters` int(11) NOT NULL,
>  `new_path` tinyint(1) DEFAULT '1',
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `cyclepathpoints`
>--
>
>DROP TABLE IF EXISTS `cyclepathpoints`;
>CREATE TABLE IF NOT EXISTS `cyclepathpoints` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `path_id` int(11) NOT NULL,
>  `start_lat` float(10,6) NOT NULL,
>  `start_long` float(10,6) NOT NULL,
>  `middle_lat` float(10,6) NOT NULL,
>  `middle_long` float(10,6) NOT NULL,
>  `end_lat` float(10,6) NOT NULL,
>  `end_long` float(10,6) NOT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `cyclereviews`
>--
>
>DROP TABLE IF EXISTS `cyclereviews`;
>CREATE TABLE IF NOT EXISTS `cyclereviews` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `player_id` int(11) NOT NULL,
>  `path_id` int(11) NOT NULL,
>  `rated` int(1) NOT NULL,
>  `rated_tags` int(1) NOT NULL,
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `sugcyclepaths`
>--
>
>DROP TABLE IF EXISTS `sugcyclepaths`;
>CREATE TABLE IF NOT EXISTS `sugcyclepaths` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `path_id` int(11) NOT NULL,
>  `player_id` int(11) NOT NULL,
>  `start_lat` float(10,6) NOT NULL,
>  `start_long` float(10,6) NOT NULL,
>  `end_lat` float(10,6) NOT NULL,
>  `end_long` float(10,6) NOT NULL,
>  `pending` tinyint(1) NOT NULL DEFAULT '1',
>  `rate` int(5) NOT NULL DEFAULT '3',
>  `created_at` datetime DEFAULT NULL,
>  `updated_at` datetime DEFAULT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `playerbalance`
>--
>
>DROP TABLE IF EXISTS `playerbalance`;
>CREATE TABLE IF NOT EXISTS `playerbalance` (
>  `uid` int(11) NOT NULL AUTO_INCREMENT,
>  `player_id` int(11) NOT NULL,
>  `balance` int(5) NOT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=23 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `pathstypes`
>--
>
>DROP TABLE IF EXISTS `pathstypes`;
>CREATE TABLE IF NOT EXISTS `pathstypes` (
>  `uid` int(5) UNSIGNED NOT NULL AUTO_INCREMENT,
>  `path_set` int(5) UNSIGNED NOT NULL,
>  `name` varchar(50) NOT NULL,
>  `set_order` int(2) UNSIGNED NOT NULL,
>  PRIMARY KEY (`uid`)
>) ENGINE=MyISAM AUTO_INCREMENT=12 DEFAULT CHARSET=utf8;


>-- --------------------------------------------------------
>
>--
>-- Table structure for table `users`
>--
>
>DROP TABLE IF EXISTS `users`;
>CREATE TABLE IF NOT EXISTS `users` (
>  `id` int(5) UNSIGNED NOT NULL AUTO_INCREMENT,
>  `username` varchar(50) NOT NULL,
>  `password` varchar(50) NOT NULL,
>  PRIMARY KEY (`id`)
>) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;


11.**Δημιουργία διαχειριστών**: Επιλέξτε την βάση *pom_db* και πατήστε στην καρτέλα *Κώδικας SQL*. Στη συνέχεια τρέξτε το παρακάτω ερώτημα SQL:
 

>INSERT  INTO `users` 
>       (`username` , `password` )
>
>VALUES ('john',  SHA1('johnPsw' ) ), 
>       ('james', SHA1('jamesPsw') ),
>       ('jim',   SHA1('jimPsw'  ) );


12.**Δημιουργία τύπων μονοπατιών**: Επιλέξτε τη βάση "pom_db* και πατήστε στην καρτέλα *Κώδικας SQL*. Στη συνέχεια τρέξτε το παρακάτω ερώτημα SQL:

>INSERT INTO `pathstypes` (`uid`, `path_set`, `name`, `set_order`) VALUES
>(1, 1, 'sidewalk', 1),
>(2, 1, 'crosswalk', 2),
>(3, 1, 'pedestrian walkway', 3),
>(4, 1, 'accessible entrance', 4),
>(5, 1, 'pedestrian bridge', 5),
>(6, 1, 'pedestrian tunnel', 6),
>(7, 1, 'trail', 7);


13.Η διεύθυνση που τρέχει ο server τοπικά είναι η Διεύθυνση IPv4 που μπορεί να βρεθεί από τις Λεπτομέριες στις Συνδέσεις Δικτύου και μετά pom, π.χ.: 192.168.0.4/pom/
14. Για να μην υπάρχει πρόβλημα απόρριψης των εισερχόμενων αιτημάτων από την εφαρμογή, ανοίξτε το αρχείο httpd.conf που βρίσκεται στον φάκελο \wamp64\bin\apache\apache2.4.17\conf και αλλάξτε το
>#   onlineoffline tag - don't remove
>  Require local

σε 

>#   onlineoffline tag - don't remove
>  Require all granted


## Κατασκευή του κώδικα απομακρυσμένα σε πακέτο web hosting

Απαιτείται αγορά domain name και πακέτου Web Hosting με υποστήριξη PHP, MySQL, HTTP post αιτημάτων από browser και από εφαρμογή Android και δυνατότητας αποθήκευσης αρχείων. Αφού συνδεθεί το Domain Name με το πακέτο, γίνεται η δημιουργία της βάσης δεδομένων κατά αντιστοιχία με όσα περιγράφηκαν παραπάνω και τέλος το ανέβασμα των αρχείων του project.




