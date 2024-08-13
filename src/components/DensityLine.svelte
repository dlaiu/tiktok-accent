<script>
    import * as d3 from "d3";
  
    export let stats;
    export let fill_code;
    export let x;
    export let y;

    // console.log(stats);
  
    const lineGenerator = d3.line()
      .curve(d3.curveBasis)
      .x(d => x(d[0]))
      .y(d => y(d[1]));
  </script>
  
  <path 
    class="line" 
    d={lineGenerator(stats)} 
    fill={fill_code}
  />
  
  <style>
    .line {
      stroke-width: 1px;
      stroke-linejoin: round;
      /* stroke: steelblue; */
      opacity: 0.6;
    }
  </style>
  