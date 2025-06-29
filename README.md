# 🛡️ Ciberseguridad | ABOREOR
¡Hola! Soy ABOREOR, especialista en ciberseguridad con enfoque en [pentesting, blue team, análisis de malware, etc.].  
<?php

$TRANSLATIONS = include __DIR__ . "/../src/translations.php";

/**
 * Get the percentage of translated phrases for each locale
 *
 * @param array $translations The translations array
 * @return array The percentage of translated phrases for each locale
 */
function getProgress(array $translations): array
{
    $phrases_to_translate = [
        "Total Contributions",
        "Current Streak",
        "Longest Streak",
        "Week Streak",
        "Longest Week Streak",
        "Present",
        "Excluding {days}",
    ];

    $translations_file = file(__DIR__ . "/../src/translations.php");
    $progress = [];
    foreach ($translations as $locale => $phrases) {
        // skip aliases
        if (is_string($phrases)) {
            continue;
        }
        $translated = 0;
        foreach ($phrases_to_translate as $phrase) {
            if (isset($phrases[$phrase])) {
                $translated++;
            }
        }
        $percentage = round(($translated / count($phrases_to_translate)) * 100);
        $locale_name = Locale::getDisplayName($locale, $locale);
        $line_number = getLineNumber($translations_file, $locale);
        $progress[$locale] = [
            "locale" => $locale,
            "locale_name" => $locale_name,
            "percentage" => $percentage,
            "line_number" => $line_number,
        ];
    }
    // sort by percentage
    uasort($progress, function ($a, $b) {
        return $b["percentage"] <=> $a["percentage"];
    });
    return $progress;
}

/**
 * Get the line number of the locale in the translations file
 *
 * @param array $translations_file The translations file
 * @param string $locale The locale
 * @return int The line number of the locale in the translations file
 */
function getLineNumber(array $translations_file, string $locale): int
{
    return key(preg_grep("/^\\s*\"$locale\"\\s*=>\\s*\\[/", $translations_file)) + 1;
}

/**
 * Convert progress to labeled badges
 *
 * @param array $progress The progress array
 * @return string The markdown for the image badges
 */
function progressToBadges(array $progress): string
{
    $per_row = 5;
    $table = "<table><tbody>";
    $i = 0;
    foreach (array_values($progress) as $data) {
        if ($i % $per_row === 0) {
            $table .= "<tr>";
        }
        $line_url = "https://github.com/DenverCoder1/github-readme-streak-stats/blob/main/src/translations.php#L{$data["line_number"]}";
        $table .= "<td><a href=\"{$line_url}\"><code>{$data["locale"]}</code></a> - {$data["locale_name"]}<br /><a href=\"{$line_url}\"><img src=\"https://progress-bar.xyz/{$data["percentage"]}\" alt=\"{$data["locale_name"]} {$data["percentage"]}%\"></a></td>";
        $i++;
        if ($i % $per_row === 0) {
            $table .= "</tr>";
        }
    }
    if ($i % $per_row !== 0) {
        while ($i % $per_row !== 0) {
            $table .= "<td></td>";
            $i++;
        }
        $table .= "</tr>";
    }
    $table .= "</tbody></table>\n";
    return $table;
}

/**
 * Update readme by replacing the content between the start and end markers
 *
 * @param string $path The path to the readme file
 * @param string $start The start marker
 * @param string $end The end marker
 * @param string $content The content to replace the content between the start and end markers
 * @return int|false The number of bytes that were written to the file, or false on failure
 */
function updateReadme(string $path, string $start, string $end, string $content): int|false
{
    $readme = file_get_contents($path);
    if (strpos($readme, $start) === false || strpos($readme, $end) === false) {
        throw new Exception("Start or end marker not found in readme");
    }
    $start_pos = strpos($readme, $start) + strlen($start);
    $end_pos = strpos($readme, $end);
    $length = $end_pos - $start_pos;
    $readme = substr_replace($readme, $content, $start_pos, $length);
    return file_put_contents($path, $readme);
}

$progress = getProgress($GLOBALS["TRANSLATIONS"]);
$badges = "\n" . progressToBadges($progress);
$update = updateReadme(
    __DIR__ . "/../README.md",
    "<!-- TRANSLATION_PROGRESS_START -->",
    "<!-- TRANSLATION_PROGRESS_END -->",
    $badges
);
exit($update === false ? 1 : 0);


## Core Competencies

**`Technical Stack`**  
<div style="margin: 5px 0;">
<img src="https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white">
<img src="https://img.shields.io/badge/Burp_Suite-FF6633?style=flat-square">
<img src="https://img.shields.io/badge/OSCP-FFFFFF?style=flat-square&logo=offensive-security&logoColor=black">
<img src="https://img.shields.io/badge/Nmap-FFFFFF?style=flat-square&logo=nmap&logoColor=black">
  <img src="https://img.shields.io/badge/wireshark-FFFFFF?style=flat-square&logo=Wireshark&logoColor=black">
<img src="https://img.shields.io/badge/Metasploit-258C78?style=flat-square">


<!-- 🌐 Social Media Badges -->
## 🌐 Redes Sociales
<div align="center" style="margin: 25px 0; background-color: #1A1A1A; padding: 25px; border-radius: 15px; box-shadow: 0 4px 8px rgba(0,0,0,0.3);">

### <img src="https://img.icons8.com/color/24/000000/youtube-play.png"/> YouTube | <img src="https://img.icons8.com/fluency/24/000000/instagram-new.png"/> Instagram | <img src="https://img.icons8.com/pulsar-line/24/threads.png"/> Threads | <img src="https://img.icons8.com/color/24/000000/stackoverflow.png"/> Stack Overflow

[![YouTube Channel](https://img.shields.io/badge/@ABOREOR1-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://www.youtube.com/@ABOREOR1)
[![Instagram Profile](https://img.shields.io/badge/@aboreor0101-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://www.instagram.com/aboreor0101/)
[![Threads Profile](https://img.shields.io/badge/@aboreor0101-000000?style=for-the-badge&logo=threads&logoColor=white&labelColor=101010)](https://www.threads.com/@aboreor0101)
[![Stack Overflow Profile](https://img.shields.io/badge/@aboreor-F58025?style=for-the-badge&logo=stackoverflow&logoColor=white&labelColor=101010)](https://stackoverflow.com/users/23592191/aboreor)
</div>

</div>
