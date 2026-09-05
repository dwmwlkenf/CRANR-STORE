const { Client, GatewayIntentBits, EmbedBuilder, ActionRowBuilder, ButtonBuilder, ButtonStyle } = require('discord.js');

// إعداد صلاحيات البوت (Intents) اللازمة لقراءة الرسائل والتفاعل
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent,
    ],
});

client.once('ready', () => {
    console.log(`✅ تم تسجيل الدخول بنجاح باسم: ${client.user.tag}`);
});

// أمر بسيط لتجربة البوت أو إرسال لوحة المتجر
client.on('messageCreate', async message => {
    // تجاهل رسائل البوتات لكي لا يحدث تكرار
    if (message.author.bot) return;

    // إذا كتب شخص أمر !store أو !متجر
    if (message.content === '!store' || message.content === '!متجر') {
        // إنشاء واجهة مرتبة (Embed) لمتجرك
        const storeEmbed = new EmbedBuilder()
            .setColor('#5865F2')
            .setTitle('🛒 مرحباً بك في CRANR STORE (كرنر ستور)')
            .setDescription('أفضل المتجر لخدمات واشتراكات ديسكورد الرقمية بأفضل الأسعار وبشكل فوري!\n\nاختر أحد الخيارات أدناه للبدء:')
            .addFields(
                { name: '✨ مميزاتنا', value: '• تسليم سريع\n• ضمان كامل\n• دعم فني متواصل' }
            )
            .setFooter({ text: 'CRANR STORE © 2026' })
            .setTimestamp();

        // إنشاء أزرار تفاعلية أسفل الرسالة
        const row = new ActionRowBuilder()
            .addComponents(
                new ButtonBuilder()
                    .setCustomId('buy_service')
                    .setLabel('🛍️ شراء خدمة')
                    .setStyle(ButtonStyle.Primary),
                new ButtonBuilder()
                    .setCustomId('open_ticket')
                    .setLabel('🎫 فتح تذكرة دعم')
                    .setStyle(ButtonStyle.Success)
            );

        await message.reply({ embeds: [storeEmbed], components: [row] });
    }
});

// التعامل مع ضغط الأزرار
client.on('interactionCreate', async interaction => {
    if (!interaction.isButton()) return;

    if (interaction.customId === 'buy_service') {
        await interaction.reply({ content: '💳 لطلب الخدمات يرجى فتح تذكرة دعم أو التواصل مع الإدارة.', ephemeral: true });
    } else if (interaction.customId === 'open_ticket') {
        await interaction.reply({ content: '🎫 تم استلام طلبك، سيتم فتح غرفة خاصة لك قريباً!', ephemeral: true });
    }
});

// ضع توكن بوتك هنا بين علامتي التنصيص
client.login('MTU0NTYzMTg1NTI3MTg3MDUxNA.GWJScF.RgTT40kzYQhwn2hoEwG8I7L4fusM0CYRq3rUFE');
