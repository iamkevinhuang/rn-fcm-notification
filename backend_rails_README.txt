# this is the script for send push notification from rails backend
# install gem from https://github.com/decision-labs/fcm
# full tutorial belongs to : https://medium.com/@nehanakrani004/fcm-push-notification-with-ruby-on-rails-64097685ce66

require 'fcm'

namespace :try do
    desc 'Trying FCM'

    task fcm: :environment do
        fcm_client = FCM.new("yourserverkey") # set your FCM_SERVER_KEY
        options = { 
            priority: 'high',
            data: { message: "Tes From Rails" },
            notification: { 
                body: 'Tes From Rails with icon',
                title: 'Hi There ! This is your new notification !',
                sound: 'default',
                icon: 'http://simpleicon.com/wp-content/uploads/icon2.png'
            }
        }
        registration_ids = ["userToken"]
        #([Array of registration ids up to 1000])
        # Registration ID looks something like: "dAlDYuaPXes:APA91bFEipxfcckxglzRo8N1SmQHqC6g8SWFATWBN9orkwgvTM57kmlFOUYZAmZKb4XGGOOL9wqeYsZHvG7GEgAopVfVupk_gQ2X5Q4Dmf0Cn77nAT6AEJ5jiAQJgJ_LTpC1s64wYBvC"
        registration_ids.each_slice(20) do |registration_id|
            response = fcm_client.send(registration_id, options)
            puts response
        end
    end
end