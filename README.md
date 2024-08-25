<?php

// Telegram Bot token
define('BOT_TOKEN', 'API token here');

// Array of admin IDs
$admin_ids = array(
    '5427934599', // zhacraawi
    '381993621', // moh faratoon
    '698885938', // hagad
    '952316723', // muslimah lady
    '897694100'  // Abdinajiib    
);

// Array of channels to send messages to
$channels = array(
    'channel1' => '@Somalilibrary',
    'channel2' => '@somalibooks',
    'channel3' => '@xikmadosom',
    'channel4' => '@Hidaayatu_Rabi',
    'channel5' => '@maanhage',
    'channel6' => '@farsamada',
    'channel7' => '@kobciyebooks',
    'channel8' => '@daren_jaceyl',
    'channel9' => '@xaqiiqadanolosha',
    'channel10' => '@Hasancaashaq',
    'channel11' => '@Diinteenna_Macaan',
    'channel12' => '@AishaAhmed99',
    'channel13' => '@dareen_side',
    'channel14' => '@Mucaashaq_Jacayl',
    'channel15' => '@qalbidhaye143',
    'channel16' => '@Albaanichannel252',
    'channel17' => '@hogaan_Dumar',
    'channel18' => '@Mohametahmed',
    'channel19' => '@Noqoxiddig',
    'channel20' => '@KobciAqoontaada2',
    'channel21' => '@Quruxdaislaamka',
    'channel22' => '@kalimakariim',
    'channel23' => '@Al_xakiim',
    'channel24' => '@FAAIDOCHANEEL',
    'channel25' => '@Dhaqan_wanaag',
    'channel26' => '@Ruuxda',
    'channel27' => '@Dhiiragaliye',
    'channel28' => '@Qalbiyadeena',
    'channel29' => '@DunnidaSuugaanta',
    'channel30' => '@Som_Motivation',
    'channel31' => '@nuxurchaneel',
    'channel32' => '@Somalienglish3',
    'channel33' => '@guuleyso11' 
);

// Function to send message to a channel with optional parse mode
function sendMessage($chat_id, $text, $media = null, $parse_mode = null, $reply_markup = null) {
    $url = 'https://api.telegram.org/bot' . BOT_TOKEN . '/sendMessage';
    $data = array(
        'chat_id' => $chat_id,
        'text' => $text
    );

    if ($media && isset($media['file_id'])) {
        $media_type = pathinfo($media['file_id'], PATHINFO_EXTENSION);
        if ($media_type == 'jpg'  $media_type == 'png'  $media_type == 'jpeg') {
            // If media is a photo
            $url = 'https://api.telegram.org/bot' . BOT_TOKEN . '/sendPhoto';
            $data['photo'] = $media['file_id'];
            $data['caption'] = $text; // Include text as caption
        } else {
            // If media is a document
            $url = 'https://api.telegram.org/bot' . BOT_TOKEN . '/sendDocument';
            $data['document'] = $media['file_id'];
            $data['caption'] = $text; // Include text as caption
        }
    }

    if ($parse_mode) {
        $data['parse_mode'] = $parse_mode;
    }

    if ($reply_markup) {
        $data['reply_markup'] = $reply_markup;
    }

    $options = array(
        'http' => array(
            'method'  => 'POST',
            'header'  => 'Content-type: application/x-www-form-urlencoded',
            'content' => http_build_query($data)
        )
    );

    $context  = stream_context_create($options);
    $result = file_get_contents($url, false, $context);

    return $result;
}

// Check if the user ID is among the admin IDs
function isAdmin($user_id) {
    global $admin_ids;
    return in_array($user_id, $admin_ids);
}

// Handle incoming messages
$update = json_decode(file_get_contents('php://input'), true);

// Parse incoming message
$message = isset($update['message']['text']) ? $update['message']['text'] : '';
$chat_id = isset($update['message']['chat']['id']) ? $update['message']['chat']['id'] : '';
$user_id = isset($update['message']['from']['id']) ? $update['message']['from']['id'] : '';

// Check if the message is sent by an admin
if (isAdmin($user_id)) {
    if (strpos($message, "/start") === 0) {
        // Handle /start command
        $response = "Kusoo dhowow! Taliyaha somali community:👇\n\n";
        $response .= "/contact - nagala soo xidhiidh ⚙️.\n";
        $response .= "/post - Adeegso si aad post usoo dhigto 💬.\n";
        $response .= "/publish - Waxay la mid tahay /post ⏩.\n";
        $response .= "/ourchannels - List of connected channels 💭\n";
        $response .= "/addchannel - Ku darso kanaalkaaga  ➕\n";
        sendMessage($chat_id, $response);
    }
    // Add more command handlers here if needed
} else {
    // Send message if user is not an admin
    sendMessage($chat_id, "Sorry, you don't have the necessary permissions to use this bot.");
}

?>
