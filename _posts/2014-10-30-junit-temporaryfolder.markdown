---
layout: post
title:  "JUnit TemporaryFolder Rule"
date:   2014-10-30 00:28:00
categories: java junit testing unittesting test
comments: yes
---

If you have ever needed to create temporary files or folders in your unit test then you know that there is lot of work to cleanup all temporary files and folders. You have to clean up even if the test fails.
To make things a lot easier we have the TemporaryFolder Rule in JUnit. It allows you to create both files and folders that JUnit guarantees to delete when the test finishes. This cleanup happens both when test pass and fail.

See the following code for example:

{% highlight groovy %}
package net.joningi.example;


import static org.hamcrest.Matchers.is;
import static org.junit.Assert.assertThat;

import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.TemporaryFolder;

import java.io.File;
import java.io.IOException;
import java.io.Writer;
import java.nio.charset.Charset;
import java.nio.file.Files;
import java.util.List;

public class ExampleTest {

    @Rule
    public TemporaryFolder tempFolder = new TemporaryFolder();

    @Test
    public void testWriteToFile() throws IOException {
        final File tempFile = tempFolder.newFile("party.txt");

        try (Writer writer = Files.newBufferedWriter(tempFile.toPath(), Charset.defaultCharset())) {
            writer.write("Party time");
        }

        final List<String> lines = Files.readAllLines(tempFile.toPath(), Charset.defaultCharset());

        assertThat(lines.get(0), is("Party time"));
    }
}
{% endhighlight %}
