const express = require("express");
const app = express();

app.get("/", (req, res) => res.send("Hello World!"));

app.listen(() => console.log(` listening at 5am shop 😮‍💨`));

const moment = require("moment");
const ms = require("ms");
const {
  Client,
  Intents,
  MessageEmbed,
  Interaction,
  MessageButton,
  MessageActionRow,
  Modal,
  WebhookClient,
  MessageSelectMenu,
  Collection,
  Permissions,
  MessageFlags,
  GatewayIntentBits,
  TextInputComponent,
  ButtonBuilder,
  ActionRowBuilder,
} = require("discord.js");
const Discord = require("discord.js");

const client = new Client({
  intents: [
    Intents.FLAGS.GUILDS,
    Intents.FLAGS.GUILD_MESSAGES,
    Intents.FLAGS.GUILD_MEMBERS,
    32767,
  ],
});
client.setMaxListeners(0);
client.login(process.env.Token);

client.on("ready", () => {
  console.log(`token`);
  client.user.setActivity(`On Top`, { type: "LISTENING" });
  client.user.setStatus("idle");
});

const db = require("pro.db");
db.backup("backup");
const EmjTrue = "<:arcane_2:1173770791930839040>";
const EmjFalse = "<a:dh:1198200862200303636>";
const OrderRoom = "1199319814997561375";
const Montagat = "1199319864792334376";
const Designer = "1199319864792334376";
const Developer = "1199319864792334376";
const StaffManager = "1199319739265200168";
const DisLeader = "1199319739265200168";
const OfficialRole = "1199319739265200168";
const RolesRole = "1199319739265200168";
const DisStaff = "1199319739265200168";
const DivID = "1199319739265200168";
const ManshoratRom = "ク・المنشورات・المميزة";
const ManshoratRoom = "1184876464164835509";
const ManshoratChannel = "ク・المنشورات・المميزة";
const prefix = "$";
const lineLink =
"https://cdn.discordapp.com/attachments/1287532598020542485/1287792225102594153/41_20240923180606.png?ex=66f2d578&is=66f183f8&hm=7a081358742e475bf27d2217c36b2e47d44a68f7cb34f94db4ef39542dda5428&";

process.on("uncaughtException", (error) => {
  console.error("Uncaught exception occurred:", error);
});
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled promise rejection:", reason);
});


// == [ Come ]

client.on("messageCreate", async (message) => {
  if (
    (message.content.startsWith("$نداء") &&
      message.member.roles.cache.has(DisStaff)) ||
    (message.content.startsWith("$come") &&
      message.member.roles.cache.has(DisStaff))
  ) {
    try {
      const channel = message.channel;
      const args = message.content.split(" ");
      const user =
        message.mentions.members.first() ||
        message.guild.members.cache.get(args[1]);
      const commandLink = `https://discord.com/channels/${message.guild.id}/${message.channel.id}/${message.id}`;
      if (!user) return message.reply("**منشن الشخص أولاً أو ضع الإيدي !**");
      await user.send(
        `** يرجى التوجه إلى ${channel} في أقرب وقت !\n  الإستدعاء من قبل : ${message.member} .\n  رسالة الإستدعاء : ${commandLink} -تعال**`,
      );
      await message.reply(
        `**${EmjTrue} لقد تم نداء ${user} إلى هذا الروم بنجاح !**`,
      );
    } catch {
      await message.reply(`**${EmjFalse} لا يمكنني ارسال رسالة لهذا الشخص !**`);
    }
  }
});

// == [ PosT ]

let manshor;
let member;

client.on("messageCreate", (message) => {
  if (message.content.startsWith(prefix + "ads")) {
    if (
      message.member.roles.cache.has(DisStaff) ||
      message.member.roles.cache.some((r) => r.id == 1199319739265200168)
    ) {
      if (message.content.startsWith(prefix + "ma")) return false;

      member = message.mentions.members.first();
      if (!member)
        return message.reply(`**${EmjFalse} يرجى منشن صاحب المنشور أولاً !**`);
      manshor = message.content.split(" ").slice(2).join(" ");
      if (!manshor)
        return message.reply(`**${EmjFalse} يرجى وضع المنشور أولاً !**`);

      let embed = new Discord.MessageEmbed()
        .setTitle(`** إختر نوع المنشن :**`)
        .setDescription(
          `** يرجى إختيار نوع المشنن المناسب : \`here\` أو \`everyone\`\n المنشور :\n\`${manshor}\`**`,
        )
        .setColor(`${colorE}`);
      let row = new Discord.MessageActionRow()
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Here")
            .setCustomId("menthere")
            .setStyle("SECONDARY"),
        )
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Everyone")
            .setCustomId("menteve")
            .setStyle("SECONDARY"),
        );

      message.reply({ embeds: [embed], components: [row] });
    }
  }
});

client.on("interactionCreate", async (interaction) => {
  if (!interaction.isButton()) return;

  if (interaction.customId === "menthere") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );

      const heremanshor = `${manshor}\n@here`;

      let embed1 = new Discord.MessageEmbed()
        .setTitle(`**هـل أنـت مـتـاكـد مـن أرسـال هـذا الـمـنـشـور ؟**`)
        .setDescription(
          `** يرجى الإستجابة مع الأزرار بـ \`Send\` أو \`Cancel\` ..\n المنشور :\n\`${heremanshor}\n\nتواصل مع : ${member}\`**`,
        )
        .setColor(`${colorE}`);
      let row1 = new Discord.MessageActionRow()
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Send")
            .setCustomId("completeid")
            .setStyle("SUCCESS"),
        )
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Cancel")
            .setCustomId("cancelid")
            .setStyle("DANGER"),
        );
      interaction.deferUpdate();

      message.edit({ embeds: [embed1], components: [row1] });
    } else {
      interaction.reply({
        content: `**${EmjFalse} لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  } else if (interaction.customId === "menteve") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      const evemanshor = `${manshor}\n@everyone`;
      let embed2 = new Discord.MessageEmbed()
        .setTitle(`** هل انت متأكد من إرسال المنشور ؟**`)
        .setDescription(
          `** يرجى الإستجابة مع الأزرار بـ \`Send\` أو \`Cancel\` ..\n المنشور :\n\`${evemanshor}\n\nتواصل مع : ${member}\`**`,
        )
        .setColor(`${colorE}`);
      let row2 = new Discord.MessageActionRow()
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Send")
            .setCustomId("completeid2")
            .setStyle("SUCCESS"),
        )
        .addComponents(
          new Discord.MessageButton()
            .setLabel("Cancel")
            .setCustomId("cancelid")
            .setStyle("DANGER"),
        );
      interaction.deferUpdate();
      message.edit({ embeds: [embed2], components: [row2] });
    } else {
      interaction.reply({
        content: `**${EmjFalse}
 لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  } else if (interaction.customId === "nomen") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      const nomenmanshor = `${manshor}`;
      let embed2 = new Discord.MessageEmbed()
        .setTitle(`** هل انت متأكد من إرسال المنشور ؟**`)
        .setDescription(
          `** يرجى الإستجابة مع الأزرار بـ \`إرسال\` أو \`إلغاء\` ..\n المنشور :\n\`${nomenmanshor}\n\nتواصل مع : ${member}\`**`,
        )
        .setColor(`${colorE}`);
      let row2 = new Discord.MessageActionRow()
        .addComponents(
          new Discord.MessageButton()
            .setLabel("إرسال")
            .setCustomId("completeid3")
            .setStyle("SUCCESS"),
        )
        .addComponents(
          new Discord.MessageButton()
            .setLabel("إلغاء")
            .setCustomId("cancelid")
            .setStyle("DANGER"),
        );
      interaction.deferUpdate();
      message.edit({ embeds: [embed2], components: [row2] });
    } else {
      interaction.reply({
        content: `**${EmjFalse} لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  }
});

client.on("interactionCreate", async (interaction) => {
  if (interaction.customId == "cancelid") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      let embed3 = new Discord.MessageEmbed().setColor(`EA3648`)
        .setDescription(`** تم إلغاء إرسال هذا المنشور .
 بواسطة :
${interaction.member}
**`);
      interaction.deferUpdate();
      message.edit({ embeds: [embed3], components: [] });
    } else {
      interaction.reply({
        content: `**${EmjFalse} لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  }
});

client.on("interactionCreate", async (interaction) => {
  if (interaction.customId == "completeid") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      const now = new Date();
      const manshoratRoom = "❃・الـمـنـشـورات・الـمـمـيـزة";
      const ManshoratChannel = interaction.guild.channels.cache.find(
        (channel) => channel.name === manshoratRoom,
      );
      const ManshoratLog = client.channels.cache.get("1199319860816126004");
      const mehere = `${member}`;
      const manshorhere = `${manshor}\n\nتواصل مع : ${mehere}\n
\`-\`||@here||`;
      let embed4 = new Discord.MessageEmbed().setColor(`${colorE}`)
        .setDescription(`** تم إرسال هذا المنشور إلى ${ManshoratRom}
 بواسطة:
${interaction.member}
**`);
      message.edit({ embeds: [embed4], components: [] });
      interaction.deferUpdate();

      // Check if ManshoratChannel is defined before sending messages
      if (ManshoratChannel) {
        await ManshoratChannel.send(`${manshorhere}`);
        await ManshoratChannel.send(
          `** منشور مدفوع نخلي كامل مسؤوليتنا للي يصير بينكم , تبي زيه حياك : ** ⁠<#1162595280881987585>`,
        );
        await ManshoratChannel.send({ files: [lineLink] });
      } else {
        console.log("໒・manshorat・log");
      }

      await ManshoratLog.send(
        `** المنشور :\n\`${manshor}\`\n المنشن :\n @here \n المشرف المسؤول :\n${
          interaction.member
        }\n صاحب المنشور :\n${mehere}\n تاريخ المنشور : <t:${Math.floor(
          now.getTime() / 1000,
        )}:d>**`,
      );
      await ManshoratLog.send(`${lineLink}`);
    } else {
      interaction.reply({
        content: `**${EmjFalse}
 لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  }
});

client.on("interactionCreate", async (interaction) => {
  if (interaction.customId == "completeid2") {
    if (interaction.member.roles.cache.some((role) => role.id === DisLeader)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      const now = new Date();
      const manshoratRoom2 = "❃・الـمـنـشـورات・الـمـمـيـزة";
      const ManshoratChannel2 = interaction.guild.channels.cache.find(
        (channel) => channel.name === manshoratRoom2,
      );
      const ManshoratLog2 = client.channels.cache.get("1199319860816126004");
      const memeve = `${member}`;
      const manshoreve = `${manshor}\n\nتواصل مع : ${memeve}\n
\`-\`||@everyone||`;
      let embed4 = new Discord.MessageEmbed().setColor(`${colorE}`)
        .setDescription(`** تم إرسال هذا المنشور إلى ${ManshoratRom}
 بواسطة:
${interaction.member}
**`);
      message.edit({ embeds: [embed4], components: [] });
      interaction.deferUpdate();

      // Check if ManshoratChannel2 is defined before sending messages
      if (ManshoratChannel2) {
        await ManshoratChannel2.send(`${manshoreve}`);
        await ManshoratChannel2.send(
          `** منشور مدفوع نخلي كامل مسؤوليتنا للي يصير بينكم , تبي زيه حياك : ** ⁠<#1199319891459715173>`,
        );
        await ManshoratChannel2.send({ files: [lineLink] });
      } else {
        console.log("໒・manshorat・log");
      }

      await ManshoratLog2.send(
        `** المنشور :\n\`${manshor}\`\n المنشن :\n @everyone \n المشرف المسؤول :\n${
          interaction.member
        }\n صاحب المنشور :\n${memeve}\n تاريخ المنشور : <t:${Math.floor(
          now.getTime() / 1000,
        )}:d>**`,
      );
      await ManshoratLog2.send(`${lineLink}`);
    } else {
      interaction.reply({
        content: `**${EmjFalse}
 لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  }
});

client.on("interactionCreate", async (interaction) => {
  if (interaction.customId == "completeid3") {
    if (interaction.member.roles.cache.some((r) => r.id == DisStaff)) {
      const message = await interaction.channel.messages.fetch(
        interaction.message.id,
      );
      const now = new Date();

      await interaction.guild.channels.fetch();

      const manshoratRoom3 = "❃・الـمـنـشـورات・الـمـمـيـزة";
      const ManshoratChannel3 = interaction.guild.channels.cache.find(
        (channel) => channel.name === manshoratRoom3,
      );
      const ManshoratLog3 = client.channels.cache.get("1199319860816126004");
      const nomen = `${member}`;
      const manshorno = `${manshor}\n\nتواصل مع: ${nomen}`;
      let embed4 = new Discord.MessageEmbed().setColor(`${colorE}`)
        .setDescription(`**تـم أرسـال الـمـنـشـور هـنـا  ${ManshoratRom}
 بواسطة:
${interaction.member}
**`);
      message.edit({ embeds: [embed4], components: [] });
      interaction.deferUpdate();

      // Check if ManshoratChannel3 is defined before sending messages
      if (ManshoratRom) {
        await ManshoratRom.send(`${manshorno}`);
        await ManshoratRom.send(
          `** منشور مدفوع نخلي كامل مسؤوليتنا للي يصير بينكم , تبي زيه حياك : ** ⁠<#1156958626251026483>`,
        );
        await ManshoratChannel3.send({ files: [lineLink] });
      } else {
        console.log("໒・manshorat・log");
      }

      await ManshoratLog3.send(
        `** المنشور :\n\`${manshor}\`\n المنشن :\nno mention\n المشرف المسؤول :\n${
          interaction.member
        }\n صاحب المنشور :\n${nomen}\n تاريخ المنشور : <t:${Math.floor(
          now.getTime() / 1000,
        )}:d>**`,
      );
      await ManshoratLog3.send(`${lineLink}`);
    } else {
      interaction.reply({
        content: `**${EmjFalse}
 لا يمكنك إستخدام هذا الزر .**`,
        ephemeral: true,
      });
    }
  }
});

// == [ TaX ]

client.on("messageCreate", async (message) => {
  let args = message.content.split(" ").slice(1).join(" ");
  if (args.endsWith("b")) {
    args = args.replace(/b/gi, "") * 1000000000;
  } else if (args.endsWith("m")) {
    args = args.replace(/m/gi, "") * 1000000;
  } else if (args.endsWith("k")) {
    args = args.replace(/k/gi, "") * 1000;
  }

  if (message.author.bot) return;
  if (!message.guild) return;
  if (
    message.content.startsWith(prefix + "tax") ||
    message.content.startsWith("ضريبة") ||
    message.content.startsWith("ضريبه") ||
    message.content.startsWith("$tax") ||
    message.content.startsWith("$ضريبه") ||
    message.content.startsWith("$ضريبة")
  ) {
    let args2 = parseInt(args);
    let tax = Math.floor(args2 * (20 / 19) + 1);
    let tax2 = Math.floor(args2 * (20 / 19) + 1 - args2);
    let tax3 = Math.floor(tax2 * (20 / 19) + 1);
    let tax4 = Math.floor(tax2 + tax3 + args2);
    let embed1 = new Discord.MessageEmbed()
      .setTitle(`**${EmjFalse} | Error \`:\`**`)
      .setDescription("**يرجى وضع المبلغ المراد حساب ضريبته .**")
      .setThumbnail(message.author.avatarURL({ dynamic: true }))
      .setColor(`${colorE}`)
      .setTimestamp()
      .setFooter("# ~ Marvel S.");
    if (!args2) return message.channel.send({ embeds: [embed1] });
    let embed2 = new Discord.MessageEmbed()
      .setTitle(`**${EmjFalse} | Error \`:\`**`)
      .setDescription("**يـرجـي وضـع الـمـبـلـغ !!**")
      .setThumbnail(message.author.avatarURL({ dynamic: true }))
      .setColor(`${colorE}`)
      .setTimestamp()
      .setFooter("# ~ Marvel S.");
    if (isNaN(args2)) return message.channel.send({ embeds: [embed2] });
    if (args2 < 1) return message.channel.send(3);
    let embed4 = new Discord.MessageEmbed()
      .setTitle(`**Tax \`:\`**`)
      .setDescription(`**1**`)
      .setThumbnail(message.author.avatarURL({ dynamic: true }))
      .setColor(`${colorE}`)
      .setTimestamp()
      .setFooter("# ~ Marvel S.");
    if (args2 == 1) return message.channel.send({ embeds: [embed4] });
    let taxmsg = new Discord.MessageEmbed()
      .setTitle(`**Tax \`:\`**`)
      .setColor(`${colorE}`)
      .setDescription(`**${tax}\n**`)
      .setFooter("# ~ Marvel S.")
      .setThumbnail(message.author.avatarURL({ dynamic: true }))
      .setTimestamp();
    if (args2 >= 50000000) {
      taxmsg.setImage(
        `https://media.discordapp.net/attachments/1186060421124333568/1199458615187210270/726488234296868874.gif?ex=65c29de2&is=65b028e2&hm=b528c2f8828ef653c6fc50d388f93a6196fa483bf68dc58dc7f542250373d05f&=`,
      );
    }
    message.channel.send({ embeds: [taxmsg] });
  }
});
