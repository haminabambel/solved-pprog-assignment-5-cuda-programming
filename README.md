Download Link: https://assignmentchef.com/product/solved-pprog-assignment-5-cuda-programming
<br>
The purpose of this assignment is to familiarize yourself with CUDA programming.

<hr>

Get the source code:

<pre><code class="shell language-shell">$ wget https://nycu-sslab.github.io/PP-f21/HW5/HW5.zip$ unzip HW5.zip -d HW5$ cd HW5</code></pre>

<h2 id="1problemstatementparallelingfractalgenerationwithcuda">1. Problem Statement: Paralleling Fractal Generation with CUDA</h2>

Following <a href="https://nycu-sslab.github.io/PP-f21/HW2/#2-part-2-parallel-fractal-generation-using-stdthread" target="_blank" rel="noopener">part 2 of HW2</a>, we are going to parallelize fractal generation by using CUDA.

Build and run the code in the <code>HW5</code> directory of the code base. (Type <code>make</code> to build, and <code>./mandelbrot</code> to run it. <code>./mandelbrot --help</code> displays the usage information.)

The following paragraphs are quoted from <a href="https://nycu-sslab.github.io/PP-f21/HW2/#2-part-2-parallel-fractal-generation-using-stdthread" target="_blank" rel="noopener">part 2 of HW2</a>.

<blockquote>

 This program produces the image file <code>mandelbrot-test.ppm</code>, which is a visualization of a famous set of complex numbers called the Mandelbrot set. [Most platforms have a <code>.ppm</code> viewer. For example, to view the resulting images, use <a href="https://github.com/stefanhaustein/TerminalImageViewer.git" target="_blank" rel="noopener">tiv</a> command (already installed) to display them on the terminal.]

 As you can see in the images below, the result is a familiar and beautiful fractal. Each pixel in the image corresponds to a value in the complex plane, and the brightness of each pixel is proportional to the computational cost of determining whether the value is contained in the Mandelbrot set. To get image 2, use the command option <code>--view 2</code>. You can learn more about the definition of the <a href="https://en.wikipedia.org/wiki/Mandelbrot_set" target="_blank" rel="noopener">Mandelbrot set</a>.

 <img decoding="async" src="https://camo.githubusercontent.com/80f2e33b4e20f3f86809c6203402dc6807b389bc/687474703a2f2f67726170686963732e7374616e666f72642e6564752f636f75727365732f6373333438762d31382d77696e7465722f617373745f696d616765732f61737374312f6d616e64656c62726f745f76697a2e6a7067" alt="Mandelbrot Set">

</blockquote>

Your job is to parallelize the computation of the images using CUDA. A starter code that spawns CUDA threads is provided in function <code>hostFE()</code>, which is located in <code>kernel.cu</code>. This function is the host front-end function that allocates the memory and launches a GPU kernel.

Currently <code>hostFE()</code> does not do any computation and returns immediately. You should add code to <code>hostFE()</code> function and finish <code>mandelKernel()</code> to accomplish this task.

The kernel will be implemented, of course, based on <code>mandel()</code> in <code>mandelbrotSerial.cpp</code>, which is shown below. You may want to customized it for your kernel implementation.

<pre><code class="c language-c">int mandel(float c_re, float c_im, int maxIteration){  float z_re = c_re, z_im = c_im;  int i;  for (i = 0; i &lt; maxIteration; ++i)  {    if (z_re * z_re + z_im * z_im &gt; 4.f)      break;    float new_re = z_re * z_re - z_im * z_im;    float new_im = 2.f * z_re * z_im;    z_re = c_re + new_re;    z_im = c_im + new_im;  }  return i;}</code></pre>

<h2 id="2requirements">2. Requirements</h2>

<ol>

 <li>You will modify only <code>kernel.cu</code>, and use it as the template.</li>

 <li>You need to implement three approaches to solve the questions:

  <ol>

   <li>Method 1: Each CUDA thread processes one pixel. Use <code>malloc</code> to allocate the host memory, and use <code>cudaMalloc</code> to allocate GPU memory. Name the file <code>kernel1.cu</code>. (<em>Note that you are not allowed to use the image input as the host memory directly</em>)</li>

   <li>Method 2: Each CUDA thread processes one pixel. Use <code>cudaHostAlloc</code> to allocate the host memory, and use <code>cudaMallocPitch</code> to allocate GPU memory. Name the file <code>kernel2.cu</code>.</li>

   <li>Method 3: Each CUDA thread processes a group of pixels. Use <code>cudaHostAlloc</code> to allocate the host memory, and use <code>cudaMallocPitch</code> to allocate GPU memory. You can try different size of the group. Name the file <code>kernel3.cu</code>.</li>

  </ol></li>

 <li><strong>Q1</strong> What are the pros and cons of the three methods? Give an assumption about their performances.</li>

 <li><strong>Q2</strong> How are the performances of the three methods? Plot a chart to show the differences among the three methods

  <ul>

   <li>for VIEW 1 and VIEW 2, and</li>

   <li>for different <code>maxIteration</code> (1000, 10000, and 100000).</li>

  </ul>You may want to measure the running time via the <code>nvprof</code> command to get a comprehensive view of performance.</li>

 <li><strong>Q3</strong> Explain the performance differences thoroughly based on your experimental results. Does the results match your assumption? Why or why not.</li>

 <li><strong>Q4</strong> Can we do even better? Think a better approach and explain it. Implement your method in <code>kernel4.cu</code>.</li>

</ol>

Answer the questions (marked with “<strong>Q1-Q4</strong>“) in a <strong>REPORT</strong> using <a href="https://hackmd.io/" target="_blank" rel="noopener">HackMD</a>. Notice that in this assignment a higher standard will be applied when grading the quality of your report.