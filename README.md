- 👋 Hi, I’m @raffipoghosya
- 👀 I’m interested in codeng
- 🌱 I’m currently learning react.js
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me in instagram _raffipoghosyan


<?php
function import_csv_page() {
    ?>
    <div class="wrap">
        <h2>CSV Ներմուծում</h2>
        <form method="post" enctype="multipart/form-data">
            <input type="file" name="csv_file" required>
            <input type="submit" name="import_csv" value="Ներմուծել CSV">
        </form>
    </div>
    <?php

    if (isset($_POST['import_csv'])) {
        handle_csv_upload();
    }
}

function handle_csv_upload() {
    if (!empty($_FILES['csv_file']['tmp_name'])) {
        $file = $_FILES['csv_file']['tmp_name'];

        $csv_data = array_map('str_getcsv', file($file));
        $headers = array_shift($csv_data);

        foreach ($csv_data as $row) {
            $post_data = array_combine($headers, $row);

            // Ստեղծում ենք գրառում
            $post_id = wp_insert_post([
                'post_title'   => sanitize_text_field($post_data['Title']),
                'post_content' => sanitize_textarea_field($post_data['Content']),
                'post_date'    => $post_data['Date'],
                'post_status'  => 'publish',
                'post_type'    => 'post',
            ]);

            if ($post_id && !is_wp_error($post_id)) {
                echo '<p>Գրառումը հաջողությամբ ավելացվեց: ' . $post_data['Title'] . '</p>';
            } else {
                echo '<p>Սխալ գրառման ներմուծման ժամանակ:</p>';
            }
        }
    } else {
        echo '<p>Խնդրում ենք վերբեռնել CSV ֆայլ:</p>';
    }
}

add_action('admin_menu', function() {
    add_menu_page(
        'CSV Ներմուծում',  // Էջի վերնագիր
        'CSV Ներմուծում',  // Մենյուի անունը
        'manage_options',  // Իրավունքները
        'import-csv',      // Slug (URL-ում երևացող անուն)
        'import_csv_page'  // Callback function
    );
});

